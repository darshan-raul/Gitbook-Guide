# Nodes

### Node - api server communication

* Kubernetes has a "hub-and-spoke" API pattern.&#x20;
* All API usage from nodes (or the pods they run) terminates at the API server.&#x20;
* **None of the other control plane components are designed to expose remote services**.&#x20;
* The API server is configured to listen for remote connections on a secure HTTPS port (typically 443) with one or more forms of client [authentication](https://kubernetes.io/docs/reference/access-authn-authz/authentication/) enabled. One or more forms of [authorization](https://kubernetes.io/docs/reference/access-authn-authz/authorization/) should be enabled, especially if [anonymous requests](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#anonymous-requests) or [service account tokens](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#service-account-tokens) are allowed.

Nodes should be provisioned with the public root certificate for the cluster such that they can connect securely to the API server along with valid client credentials

Pods that wish to connect to the API server can do so securely by leveraging a service account so that Kubernetes will automatically inject the public root certificate and a valid bearer token into the pod when it is instantiated.&#x20;

The `kubernetes` service (in `default` namespace) is configured with a virtual IP address that is redirected (via `kube-proxy`) to the HTTPS endpoint on the API server.



**The connections from the API server to the kubelet are used for:**

* Fetching logs for pods.
* Attaching (usually through `kubectl`) to running pods.
* Providing the kubelet's port-forwarding functionality.
