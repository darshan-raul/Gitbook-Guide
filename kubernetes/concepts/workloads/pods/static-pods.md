# Static pods

In Kubernetes, **static Pods** are Pods managed directly by the `kubelet` daemon on a specific node, bypassing the Kubernetes API server. **This means they are not scheduled by the control plane and are not subject to typical Kubernetes management features like Deployments or DaemonSets**.

***

#### 🔧 Key Characteristics of Static Pods

* **Node-specific**: <mark style="color:purple;">**Static Pods are tied to the node where their manifest file resides. They cannot be scheduled across multiple nodes automatically**</mark>.
* **Managed by kubelet**: <mark style="color:purple;">The</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`kubelet`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">monitors a designated directory (commonly</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`/etc/kubernetes/manifests`</mark><mark style="color:purple;">) for Pod manifest files</mark>. Upon detecting a manifest, it creates and manages the Pod accordingly.
* **Mirror Pods**: For visibility, the `kubelet` creates a corresponding "mirror Pod" on the API server. While these mirror Pods appear in `kubectl get pods` outputs, they cannot be controlled via standard Kubernetes commands.
* **Limited Feature Support**: <mark style="color:red;">Static Pods do not support certain Kubernetes features, such as referencing other API objects (e.g., ConfigMaps, Secrets) or using ephemeral containers.</mark>

***

#### 🛠️ Use Cases

Static Pods are primarily used for critical system components that need to run before the Kubernetes control plane is fully operational. **For instance, when initializing a cluster with `kubeadm`, components like the API server and controller manager are deployed as static Pods.**

***

#### 📌 Managing Static Pods

* **Creation**: Place a Pod manifest file in the `kubelet`'s monitored directory (`staticPodPath`).
* **Modification**: Edit the manifest file directly on the node. Changes are detected by the `kubelet`, which updates the Pod accordingly.
* **Deletion**: Remove the manifest file from the directory. The `kubelet` will terminate the corresponding Pod.

***

#### ⚠️ Considerations

While static Pods offer a way to run essential services independently of the Kubernetes control plane, they come with limitations. They lack the flexibility and features provided by standard Kubernetes objects. For workloads that require deployment across multiple nodes or integration with Kubernetes features, using controllers like DaemonSets is recommended.

***

For a comprehensive guide on creating and managing static Pods, refer to the official Kubernetes documentation: ([Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/?utm_source=chatgpt.com)).
