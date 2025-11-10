# private image registry

## Quick concept recap

* `imagePullSecrets` is a Kubernetes mechanism that lets the kubelet authenticate to an image registry when pulling container images.
* Secrets are namespaced. A secret in namespace `ns1` cannot be used by pods in `ns2`.
* You can attach `imagePullSecrets` directly to a Pod (or Deployment) or to a ServiceAccount so all pods using that SA inherit the secret.
* Preferred secret type: `kubernetes.io/dockerconfigjson` (contains `.dockerconfigjson`) or you can use `kubectl create secret docker-registry` which creates that type for you.

***

## 1) Docker Hub — simple username/password (or PAT) example

#### Create secret (recommended: `kubectl create secret docker-registry`)

```bash
kubectl create secret docker-registry dockerhub-creds \
  --docker-username=MY_DHUB_USERNAME \
  --docker-password=MY_DHUB_PASSWORD_OR_PAT \
  --docker-email=you@example.com \
  -n my-namespace
```

This creates a secret named `dockerhub-creds` in `my-namespace` of type `kubernetes.io/dockerconfigjson`.

#### Pod/Deployment example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dockerhub-pod
  namespace: my-namespace
spec:
  containers:
  - name: app
    image: docker.io/myusername/my-private-image:latest
    imagePullPolicy: IfNotPresent
  imagePullSecrets:
  - name: dockerhub-creds
```

#### Alternative: create from local `~/.docker/config.json`

If you already `docker login`ed locally:

```bash
kubectl create secret generic dockerhub-creds \
  --from-file=.dockerconfigjson=$HOME/.docker/config.json \
  --type=kubernetes.io/dockerconfigjson \
  -n my-namespace
```

#### Notes for Docker Hub

* If you use 2FA on Docker Hub, use a Personal Access Token as the password.
* Rate limits on anonymous pulls: using an authenticated secret avoids stricter rate limits for public images.

***

## 2) Google Container Registry (GCR) — service account JSON or GKE Workload Identity

### Option A — short: service account JSON (works anywhere)

1. Create a Google service account with `roles/storage.objectViewer` for the GCR bucket.
2. Download the JSON key file `gcr-key.json`.

Create the secret:

```bash
kubectl create secret docker-registry gcr-json-key \
  --docker-server=https://gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat gcr-key.json)" \
  --docker-email=you@example.com \
  -n my-namespace
```

Pod example:

```yaml
spec:
  containers:
  - name: app
    image: gcr.io/my-project/my-private-image:tag
  imagePullSecrets:
  - name: gcr-json-key
```

> Kubernetes uses username `_json_key` and the JSON content as password for GCR.

### Option B — (recommended for GKE) Workload Identity (no secret needed)

* On GKE, use Workload Identity so your pods assume a GCP service account and kubelet gets tokens automatically. This avoids storing JSON keys in k8s secrets.
* Steps: create GCP service account, map it to a Kubernetes service account, grant it `roles/storage.objectViewer`, annotate the k8s SA. (I can give exact commands for your cluster if you want.)

#### Notes for GCR

* JSON keys are long-lived credentials — prefer Workload Identity on GKE.
* `gcr.io`, `us.gcr.io`, `eu.gcr.io`, `asia.gcr.io` are common registry hosts; set `--docker-server` accordingly.

***

## 3) Amazon Elastic Container Registry (ECR)

ECR uses short-lived login tokens. The standard approach is to create a docker-registry secret and refresh it periodically, or use IAM-based alternatives (IRSA) on EKS.

### Create secret using `aws` CLI (token-based)

```bash
# region & account
REGION=us-east-1
ACCOUNT_ID=123456789012
REPO_HOST=${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com
kubectl create secret docker-registry ecr-creds \
  --docker-server=${REPO_HOST} \
  --docker-username=AWS \
  --docker-password="$(aws ecr get-login-password --region ${REGION})" \
  --docker-email=you@example.com \
  -n my-namespace
```

* `aws ecr get-login-password` returns the token. TTL: ECR tokens expire (commonly 12 hours), so you will need to refresh this secret periodically.

### Pod example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ecr-deploy
  namespace: my-namespace
spec:
  replicas: 1
  selector: { matchLabels: { app: ecr-app } }
  template:
    metadata:
      labels: { app: ecr-app }
    spec:
      containers:
      - name: app
        image: 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-image:latest
      imagePullSecrets:
      - name: ecr-creds
```

### Better option on EKS: IRSA (IAM Roles for Service Accounts)

* On EKS, prefer IRSA: attach an IAM role to a Kubernetes ServiceAccount and grant `ecr:GetAuthorizationToken` / `ecr:BatchGetImage`/`ecr:GetDownloadUrlForLayer`. The AWS EKS credential provider handles auth for you — no secret to rotate.
* Otherwise use a small controller/cronjob to refresh the secret every \~6–12 hours.

***

## 4) Using `imagePullSecrets` via ServiceAccount (recommended when many pods)

Attach the secret to a ServiceAccount so pods that use that SA automatically get the secret:

```bash
kubectl patch serviceaccount default \
  -p '{"imagePullSecrets":[{"name":"dockerhub-creds"}]}' \
  -n my-namespace
```

Or in YAML:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-sa
  namespace: my-namespace
imagePullSecrets:
- name: dockerhub-creds
```

Then pods that set `serviceAccountName: my-sa` will automatically use the secret.

***

## 5) Creating a `dockerconfigjson` secret from scratch (YAML)

If you need a YAML secret (e.g., CI creating it), build the `.dockerconfigjson` content:

`.dockerconfigjson` structure:

```json
{
  "auths": {
    "https://index.docker.io/v1/": {
      "username": "MYUSER",
      "password": "MYPASS_OR_TOKEN",
      "auth": "BASE64(username:password)",
      "email": "you@example.com"
    }
  }
}
```

Then base64-encode that JSON and put it in a k8s Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: docker-reg
  namespace: my-namespace
type: kubernetes.io/dockerconfigjson
data:
  .dockerconfigjson: <base64-of-the-json-above>
```

***

## 6) Secret rotation & automation

* **ECR tokens**: expire — automate refresh via:
  * A Kubernetes CronJob that runs `aws ecr get-login-password` and runs `kubectl create secret ... --dry-run=client -o yaml | kubectl apply -f -`.
  * Or use AWS EKS IAM authenticator/IRSA to avoid secrets.
* **GCR**: prefer Workload Identity on GKE to avoid JSON keys in k8s.
* **Docker Hub**: rotate tokens/PATs periodically.
* When you update a secret, new pod pulls will use it; existing running pods won't be re-pulled unless restarted.

Example refresh (bash snippet):

```bash
# refresh ecr secret
aws ecr get-login-password --region $REGION | \
kubectl create secret docker-registry ecr-creds \
  --docker-server=$REPO_HOST \
  --docker-username=AWS \
  --docker-password-stdin \
  --dry-run=client -o yaml | kubectl apply -f -
```

***

## 7) Troubleshooting tips

* `kubectl describe pod PODNAME` — look for `Failed to pull image`, `ErrImagePull`, `ImagePullBackOff` and the detailed message.
* Check secret exists in same namespace: `kubectl get secret -n my-namespace`.
* Check secret type: `kubectl get secret dockerhub-creds -n my-namespace -o yaml` should show `type: kubernetes.io/dockerconfigjson`.
* Ensure `image` field uses correct fully-qualified image name for the registry.
* For ECR/GCR: check IAM permissions (service account or user used to create secret must have permission to fetch token or read the registry).

***

## 8) Security best practices

* Avoid long-lived JSON keys in k8s; prefer cloud-native identity (Workload Identity, IRSA).
* Put secrets in the same namespace and limit RBAC access to them.
* Use short-lived tokens where possible and automate rotation.
* Audit pod/serviceaccount usage to avoid secret leakage.
* Use `imagePullPolicy: IfNotPresent` or pinned image digests to control unexpected re-pulls.

***

## 9) Quick reference commands

Docker Hub:

```bash
kubectl create secret docker-registry dockerhub-creds \
  --docker-username=USER \
  --docker-password=PASSWORD_OR_PAT \
  --docker-email=you@example.com -n NAMESPACE
```

GCR (JSON key):

```bash
kubectl create secret docker-registry gcr-json-key \
  --docker-server=https://gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat gcr-key.json)" \
  --docker-email=you@example.com -n NAMESPACE
```

ECR (token):

```bash
aws ecr get-login-password --region REGION | \
kubectl create secret docker-registry ecr-creds \
  --docker-server=${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com \
  --docker-username=AWS \
  --docker-password-stdin \
  -n NAMESPACE
```

***

