# Finalizers



Kubernetes finalizers are a powerful mechanism that allow you to control the deletion lifecycle of resources. They ensure that specific cleanup operations are completed before a resource is permanently removed from the cluster.([Zesty](https://zesty.co/finops-glossary/kubernetes-finalizers/?utm_source=chatgpt.com))

***

#### 🔍 What Are Finalizers?

Finalizers are strings added to the `metadata.finalizers` field of a Kubernetes object. They act as pre-delete hooks, signaling that certain tasks must be completed before the resource can be deleted. When you attempt to delete an object with finalizers:([Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/?utm_source=chatgpt.com), [Zesty](https://zesty.co/finops-glossary/kubernetes-finalizers/?utm_source=chatgpt.com), [Medium](https://praneethreddybilakanti.medium.com/1-7-mastering-finalizers-in-kubernetes-99e93bdb22ce?utm_source=chatgpt.com))

1. Kubernetes sets a `deletionTimestamp` on the object, marking it for deletion.
2. The object remains in a "terminating" state and is not immediately removed.
3. Controllers or operators responsible for the finalizer perform necessary cleanup tasks.
4. Once cleanup is complete, the finalizer is removed from the object.
5. When all finalizers are removed, Kubernetes proceeds to delete the object.([Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/?utm_source=chatgpt.com), [Medium](https://praneethreddybilakanti.medium.com/1-7-mastering-finalizers-in-kubernetes-99e93bdb22ce?utm_source=chatgpt.com))

This mechanism prevents premature deletion and ensures that dependent resources or external systems are properly handled before the object is gone. ([Medium](https://medium.com/better-programming/stop-messing-with-kubernetes-finalizers-b849511b2329?utm_source=chatgpt.com))

***

#### 🛠️ How to Use Finalizers

**Adding a Finalizer:**

You can add a finalizer when creating a resource or by updating an existing one. Here's an example of adding a finalizer to a ConfigMap:([Zesty](https://zesty.co/finops-glossary/kubernetes-finalizers/?utm_source=chatgpt.com))

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-configmap
  finalizers:
    - example.com/finalizer
data:
  key: value
```

**Handling Finalizers in Controllers:**

If you're developing a custom controller (e.g., using Kubebuilder), you can manage finalizers in your reconcile loop:

1. **Add Finalizer:** When the object is created and doesn't have your finalizer, add it.
2. **Handle Deletion:** If the object has a `deletionTimestamp` and your finalizer is present, perform cleanup tasks.
3. **Remove Finalizer:** After successful cleanup, remove your finalizer to allow deletion to proceed.([Kubebuilder](https://kubebuilder.io/reference/using-finalizers?utm_source=chatgpt.com), [Medium](https://medium.com/%40diliprao/kubernetes-finalizers-demystified-47dc72ac67ab?utm_source=chatgpt.com), [Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/?utm_source=chatgpt.com))

This pattern ensures that your controller can clean up external resources or perform other necessary actions before the Kubernetes object is deleted. ([Medium](https://medium.com/better-programming/stop-messing-with-kubernetes-finalizers-b849511b2329?utm_source=chatgpt.com))

***

#### 🧪 Common Use Cases

* **Persistent Volumes:** Finalizers like `kubernetes.io/pv-protection` prevent deletion of PersistentVolume objects that are still in use.
* **Custom Resources:** Operators managing external resources (e.g., cloud infrastructure) use finalizers to ensure those resources are deleted before the Kubernetes object is removed.
* **Namespace Deletion:** Finalizers can delay namespace deletion until all resources within the namespace are cleaned up.([Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/?utm_source=chatgpt.com))

***

#### ⚠️ Important Considerations

* **Stuck Resources:** If a finalizer is not removed (e.g., due to a controller crash), the resource will remain in a terminating state indefinitely.
* **Manual Removal:** While you can manually remove finalizers to force deletion, this should be done with caution, as it may leave external resources orphaned.
* **Idempotency:** Ensure that your cleanup logic is idempotent, meaning it can be safely retried without adverse effects.([Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/?utm_source=chatgpt.com), [Kubebuilder](https://kubebuilder.io/reference/using-finalizers?utm_source=chatgpt.com))

***

#### 📚 Further Reading

* [Kubernetes Documentation: Finalizers](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/)
* [Using Finalizers to Control Deletion](https://kubernetes.io/blog/2021/05/14/using-finalizers-to-control-deletion/)
* [Kubebuilder Book: Using Finalizers](https://book.kubebuilder.io/reference/using-finalizers)

Finalizers are a crucial tool for managing resource lifecycles in Kubernetes, especially when dealing with external systems or complex dependencies. By properly implementing and handling finalizers, you can ensure that resources are cleaned up safely and predictably.
