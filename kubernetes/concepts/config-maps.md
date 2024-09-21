# Config maps

{% embed url="https://kubernetes.io/docs/concepts/configuration/configmap/" %}

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:

1. Inside a container command and args
2. Environment variables for a container
3. Add a file in read-only volume, for the application to read
4. Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap

### Gotchas

* ConfigMap does not provide secrecy or encryption.
* ConfigMaps **consumed as environment variables** are not updated automatically and **require a pod restart.**
  * When a ConfigMap currently consumed in a volume is updated, projected keys are eventually updated as well
    * A container using a ConfigMap as a [subPath](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) volume mount will not receive ConfigMap updates.
* A ConfigMap is not designed to hold large chunks of data. The data stored in a ConfigMap cannot exceed 1 MiB
* The name of a ConfigMap must be a valid [DNS subdomain name](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#dns-subdomain-names).
* The Pod and the ConfigMap **must be in the same** [**namespace**](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces)**.**
  * You can bypass this and access a ConfigMap in a different namespace by accessing cm using k8s api
* The `spec` of a [static Pod](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/) cannot refer to a ConfigMap
* A ConfigMap doesn't differentiate between single line property values and multi-line file-like values. What matters is how Pods and other objects consume those values.

### Immutable Configmap

&#x20;you can add an `immutable` field to a ConfigMap definition to create an [immutable ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/#configmap-immutable).

{% embed url="https://kubernetes.io/docs/concepts/configuration/configmap/#configmap-immutable" %}
