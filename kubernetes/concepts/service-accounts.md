# Service Accounts

#### Basics of Kubernetes Service Accounts

1. **Introduction**
   * Service Accounts (SAs) in Kubernetes are used to provide an identity to processes running in a Pod.
   * They are distinct from normal user accounts and are intended for use by applications.
2. **Creating a Service Account**
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
3. **Associating a Service Account with a Pod**
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
4. **Service Account Tokens**
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

