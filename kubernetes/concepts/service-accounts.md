# Service Accounts



**What is a Service Account?**

* In Kubernetes, there are two main types of users: **Human Users** and **Service Accounts**.<sup>1</sup>
* **Human Users** are typically managed externally (e.g., via certificates, LDAP, or integration with identity providers like Google Accounts or AWS IAM).
* **Service Accounts** are designed for **processes** running in pods that need to access the Kubernetes API, not for human administrators. They provide an identity for workloads.
* Service Accounts are namespaced resources within Kubernetes. A default Service Account is automatically created in every namespace.

**What is a Service Account Token?**

* A Service Account Token is a credential (specifically, a type of [JSON Web Token - JWT](https://jwt.io/)) that a process inside a pod uses to authenticate its identity to the Kubernetes API server.
* When a pod runs, it is associated with a Service Account.<sup>2</sup> By default, this is the `default` Service Account in its namespace, but you can specify a different one in the pod's specification (`serviceAccountName`).
* If a pod is configured to _automount_ its Service Account token (which is the default behavior unless disabled), Kubernetes makes the Service Account's token available to the processes inside the pod, typically mounted as a file in the pod's filesystem (by default, at `/var/run/secrets/kubernetes.io/serviceaccount/token`).

**How do they work?**

1. A pod is created and associated with a Service Account.
2. Kubernetes ensures the Service Account's token is available inside the pod's filesystem (either automatically mounted or via a projected volume).<sup>3</sup>
3. The application running inside the pod reads the token from the designated file path.<sup>4</sup>
4. When the application makes a request to the Kubernetes API server (e.g., to list pods, create a deployment, etc.), it includes this token in the `Authorization` header, typically as a Bearer Token (`Authorization: Bearer <token>`).<sup>5</sup>
5. The Kubernetes API server receives the request and the token. It verifies the token's signature (using a signing key configured on the API server) and validates its contents (claims) to identify the Service Account and potentially check its audience or expiry.
6. Based on the authenticated Service Account identity, Kubernetes's Authorization layer (commonly RBAC - Role-Based Access Control) determines if the Service Account has the necessary permissions to perform the requested action.

**Types of Service Account Tokens (Evolution):**

Kubernetes has evolved how Service Account tokens are managed to improve security:

1. **Legacy/Secret-Based Tokens (Deprecated for new clusters):**
   * Historically, a default, non-expiring token was automatically created as a `Secret` object for every Service Account.
   * These tokens were essentially static secrets that didn't expire unless the associated Secret was manually deleted.
   * This approach had security drawbacks because a compromised token could be used indefinitely.<sup>6</sup>
   * While still present for backward compatibility in many clusters, creating these automatically for new Service Accounts is being phased out.
2. **Bound Service Account Tokens (Recommended):**
   * This is the modern, more secure approach.
   * These tokens have a configurable **time-bound lifespan** (they expire).<sup>7</sup>
   * They can be **audience-bound**, meaning they are intended for a specific recipient or service (e.g., the Kubernetes API server, or a specific external service configured to accept them).<sup>8</sup> This limits their usability if leaked.
   * They are typically delivered to pods using a **projected volume** (`projected` type in the Pod spec), rather than being stored as static Secrets.<sup>9</sup> This allows the kubelet to fetch and rotate the token periodically within the pod's volume.
   * Clusters are moving towards defaulting to bound tokens and discouraging the use of the legacy Secret-based tokens.

**Key Considerations and Best Practices:**

* **RBAC is Essential:** Service Accounts themselves have no permissions by default. You _must_ grant permissions to a Service Account using Kubernetes RBAC (Roles and RoleBindings or ClusterRoles and ClusterRoleBindings) for it to be able to perform any actions on the API. Grant only the minimum permissions required (Principle of Least Privilege).
* **Disable Automounting When Not Needed:** For pods that do _not_ need to communicate with the Kubernetes API, set `automountServiceAccountToken: false` in the pod spec or the Service Account itself.<sup>10</sup> This reduces the attack surface.
* **Use Bound Tokens and Projected Volumes:** Prefer the modern bound tokens delivered via projected volumes for better security due to their limited lifespan and audience binding.
* **Token Signing Key Rotation:** The private key used by the API server to sign Service Account tokens is a critical credential. Kubernetes supports rotating this key without invalidating existing tokens immediately, which is a good security practice.



1. **Creating a Service Account**
   * Service Accounts can be created using `kubectl` or directly through a YAML manifest.
   *   Example using `kubectl`:

       ```sh
       kubectl create serviceaccount my-service-account
       ```
   *   Example using YAML:

       ```yaml
       apiVersion: v1
       kind: ServiceAccount
       metadata:
         name: my-service-account
       ```
2. **Associating a Service Account with a Pod**
   * Specify the Service Account in the Pod specification.
   *   Example:

       ```yaml
       apiVersion: v1
       kind: Pod
       metadata:
         name: my-pod
       spec:
         serviceAccountName: my-service-account
         containers:
         - name: my-container
           image: my-image
       ```
3. **Service Account Tokens**
   * When a Pod is created, Kubernetes automatically mounts a Service Account token into the Pod at `/var/run/secrets/kubernetes.io/serviceaccount/token`.
   * This token can be used to authenticate with the Kubernetes API.

#### Advanced Topics

1. **Role-Based Access Control (RBAC)**
   * RBAC is used to manage permissions for Service Accounts.
   *   Example of creating a Role and RoleBinding:

       ```yaml
       apiVersion: rbac.authorization.k8s.io/v1
       kind: Role
       metadata:
         namespace: default
         name: pod-reader
       rules:
       - apiGroups: [""]
         resources: ["pods"]
         verbs: ["get", "watch", "list"]
       ```

       ```yaml
       apiVersion: rbac.authorization.k8s.io/v1
       kind: RoleBinding
       metadata:
         name: read-pods
         namespace: default
       subjects:
       - kind: ServiceAccount
         name: my-service-account
         namespace: default
       roleRef:
         kind: Role
         name: pod-reader
         apiGroup: rbac.authorization.k8s.io
       ```
2. **ClusterRoles and ClusterRoleBindings**
   * For permissions that span across the entire cluster, use ClusterRoles and ClusterRoleBindings.
   *   Example:

       ```yaml
       apiVersion: rbac.authorization.k8s.io/v1
       kind: ClusterRole
       metadata:
         name: cluster-admin-role
       rules:
       - apiGroups: [""]
         resources: ["pods"]
         verbs: ["get", "list", "watch", "create", "delete"]
       ```

       ```yaml
       apiVersion: rbac.authorization.k8s.io/v1
       kind: ClusterRoleBinding
       metadata:
         name: cluster-admin-binding
       subjects:
       - kind: ServiceAccount
         name: my-service-account
         namespace: default
       roleRef:
         kind: ClusterRole
         name: cluster-admin-role
         apiGroup: rbac.authorization.k8s.io
       ```
3. **Service Account Automation**
   * Tools like Helm and Kustomize can be used to manage Service Accounts and their bindings as part of your deployment configurations.
4. **Service Account Security**
   * Limit the permissions of Service Accounts to the minimum required.
   * Rotate Service Account tokens periodically.
   * Monitor and audit the usage of Service Accounts.

#### Example: Secure Application with Service Account

1.  **Create a Service Account**

    ```yaml
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: secure-app-sa
    ```
2.  **Create a Role with limited permissions**

    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: Role
    metadata:
      name: secure-app-role
      namespace: default
    rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["get", "list"]
    ```
3.  **Bind the Role to the Service Account**

    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: secure-app-binding
      namespace: default
    subjects:
    - kind: ServiceAccount
      name: secure-app-sa
      namespace: default
    roleRef:
      kind: Role
      name: secure-app-role
      apiGroup: rbac.authorization.k8s.io
    ```
4.  **Deploy a Pod with the Service Account**

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: secure-app-pod
    spec:
      serviceAccountName: secure-app-sa
      containers:
      - name: secure-app-container
        image: my-secure-app-image
    ```

***

## To access custom resources



To allow a Service Account to list custom resources, such as Crossplane resources, you need to create a Role or ClusterRole with the appropriate permissions and bind it to the Service Account using a RoleBinding or ClusterRoleBinding.

Here’s a step-by-step guide to achieve this:

#### Step 1: Identify the Custom Resource Definitions (CRDs)

First, identify the CRDs you want the Service Account to list. For Crossplane, you might have CRDs like `compositions`, `compositeresourcedefinitions`, etc.

You can list the available CRDs in your cluster with:

```sh
kubectl get crds
```

#### Step 2: Create a Role or ClusterRole

Create a Role or ClusterRole that grants the Service Account permission to list the desired custom resources.

**Role (Namespace-Scoped)**

If the custom resources are namespace-scoped, create a Role:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default  # Change to the appropriate namespace
  name: crossplane-resource-reader
rules:
- apiGroups: ["apiextensions.crossplane.io"]  # Change to the appropriate API group
  resources: ["compositions", "compositeresourcedefinitions"]  # List the specific custom resources
  verbs: ["get", "list", "watch"]
```

**ClusterRole (Cluster-Scoped)**

If the custom resources are cluster-scoped or if you want to grant permissions across all namespaces, create a ClusterRole:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: crossplane-resource-reader
rules:
- apiGroups: ["apiextensions.crossplane.io"]  # Change to the appropriate API group
  resources: ["compositions", "compositeresourcedefinitions"]  # List the specific custom resources
  verbs: ["get", "list", "watch"]
```

#### Step 3: Create a RoleBinding or ClusterRoleBinding

Bind the Role or ClusterRole to the Service Account.

**RoleBinding (for Role)**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: crossplane-resource-binding
  namespace: default  # Change to the appropriate namespace
subjects:
- kind: ServiceAccount
  name: my-service-account  # Name of your Service Account
  namespace: default  # Namespace of your Service Account
roleRef:
  kind: Role
  name: crossplane-resource-reader
  apiGroup: rbac.authorization.k8s.io
```

**ClusterRoleBinding (for ClusterRole)**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: crossplane-resource-binding
subjects:
- kind: ServiceAccount
  name: my-service-account  # Name of your Service Account
  namespace: default  # Namespace of your Service Account
roleRef:
  kind: ClusterRole
  name: crossplane-resource-reader
  apiGroup: rbac.authorization.k8s.io
```

#### Step 4: Apply the YAML Manifests

Save the above YAML configurations to files (e.g., `role.yaml`, `rolebinding.yaml`) and apply them using `kubectl`:

```sh
kubectl apply -f role.yaml
kubectl apply -f rolebinding.yaml
```

#### Example

Here is a complete example:

**Service Account**

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: default
```

**ClusterRole**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: crossplane-resource-reader
rules:
- apiGroups: ["apiextensions.crossplane.io"]
  resources: ["compositions", "compositeresourcedefinitions"]
  verbs: ["get", "list", "watch"]
```

**ClusterRoleBinding**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: crossplane-resource-binding
subjects:
- kind: ServiceAccount
  name: my-service-account
  namespace: default
roleRef:
  kind: ClusterRole
  name: crossplane-resource-reader
  apiGroup: rbac.authorization.k8s.io
```

#### Apply Manifests

```sh
kubectl apply -f serviceaccount.yaml
kubectl apply -f clusterrole.yaml
kubectl apply -f clusterrolebinding.yaml
```

