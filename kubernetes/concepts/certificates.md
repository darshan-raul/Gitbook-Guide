# Certificates

#### Listing all the necessary certificates

This gives us an overview of all the certificates that we need to prepare for a fully functioning Kubernetes cluster:

* `etcd` peer certificate, for every `etcd` instance
* server certificates:
  * `etcd` server certificate, for every `etcd` instance
  * `kube-apiserver` server certificate
  * `kubelet` server certificate
* client certificates
  * `kube-apiserver` client certificate to communicate with `etcd`
  * `kube-apiserver` client certificate to communicate with `kubelet`s
  * `kubelet` client certificate to communicate with `kube-apiserver`, for every control and worker node
  * `kube-scheduler` client certificate to communicate with `kube-apiserver`
  * `kube-controller-manager` client certificate to communicate with `kube-apiserver`
  * `kube-proxy` client certificate to communicate with `kube-apiserver`
  * client certificates for human users to communicate with the Kubernetes API (`kube-apiserver`)
* certificate and key for verifying and signing service account tokens

Of course, every certificate must be signed by a Certificate Authority



Additional ones if needed

* **Front Proxy Client and Server Certificates (for API Aggregation):**
  * If you are using API aggregation (e.g., for custom resources served by an extension API server), the `kube-apiserver` acts as a front proxy. It needs a **front-proxy client certificate** to authenticate itself to the extension API servers.
  * The extension API servers also need a **front-proxy CA certificate** in their trust store to verify the client certificate presented by the `kube-apiserver`.
  * The extension API servers themselves will also need their own **server certificates** to authenticate to the `kube-apiserver` when the `kube-apiserver` connects to them.
* **Admission Webhook Certificates:**
  * If you use Validating or Mutating Admission Webhooks, the API server needs to securely communicate with the webhook service.
  * The webhook server needs a **server certificate** to authenticate itself to the API server.
  * The API server needs to trust the CA that signed the webhook server certificate (often configured via the `admissionregistration.k8s.io/v1` API objects).
* **Cloud Controller Manager Client Certificate:**
  * If you are running a Cloud Controller Manager outside of the `kube-controller-manager` process (common in cloud provider integrations), it will need a **client certificate** to communicate with the `kube-apiserver`.
* **OpenID Connect (OIDC) Certificates (if used for authentication):**
  * If you configure the API server to authenticate users via an OIDC provider, the API server will need to trust the CA that signed the OIDC provider's certificates. This isn't a certificate _for_ Kubernetes components but is necessary for a secure authentication flow.
