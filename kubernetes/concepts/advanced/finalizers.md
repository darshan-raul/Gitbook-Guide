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



***

## Examples



Here are **real, production-grade examples of finalizers** used across core Kubernetes resources. These are finalizers you will actually see in real clusters and the scenarios they protect.

***

## ✅ 1. PersistentVolume (PV) Finalizers

#### Finalizers:

* **`kubernetes.io/pv-protection`**

#### Scenario:

If a PV is still being used by a PVC (bound), Kubernetes must **prevent accidental deletion** of the PV object.\
This avoids losing storage for a running workload.

#### How it is handled:

1.  User runs:

    ```bash
    kubectl delete pv mypv
    ```
2. Kubernetes adds a `deletionTimestamp`, but the finalizer blocks deletion while **PVC is still bound**.
3. Only after the PV is safely released/unbound, the controller removes the finalizer.
4. PV is fully deleted.

**Why?**\
Without this, deleting a PV while a Pod is using it could instantly wipe disk data from the cluster’s view (and in cloud environments trigger storage deletion).

***

## ✅ 2. PersistentVolumeClaim (PVC) Finalizers

#### Finalizers:

* **`kubernetes.io/pvc-protection`**

#### Scenario:

Prevents a PVC from being deleted while:

* a Pod still references it, or
* the namespace is being torn down but Pods aren’t terminated yet.

#### How handled:

* Finalizer stays until **ALL Pods using the PVC are deleted / detached**.
* Then finalizer is removed, PVC can be deleted.

***

## ✅ 3. Namespace Finalizers

#### Finalizers:

* **`kubernetes`**
* **`kubernetes.io/metadata-protection`**
* **Custom finalizers from operators inside the namespace**

#### Scenario:

Namespace deletion must wait until **all resources inside the namespace are deleted**.

#### How handled:

1. User deletes namespace.
2. Namespace goes into **Terminating**.
3. The namespace controller iterates over all objects (Deployments, PVCs, CRDs…)
4. Only when **everything is successfully removed**, Kubernetes removes the namespace’s finalizers.

**Problem scenario:**\
A CRD stuck with a bad finalizer → the namespace becomes “stuck terminating”.

***

## ✅ 4. Deployments / ReplicaSets Finalizers

Most built-in workloads **do not** add finalizers themselves.\
But **Deployer controllers** may add these:

#### Common finalizers:

* `apps.openshift.io/deployment-finalizer` (OpenShift)
* `controller.kubernetes.io/pod-garbage-collector`

#### Scenario:

Ensure things like:

* All child ReplicaSets are cleaned up
* Pods are garbage-collected properly

***

## ✅ 5. Services Finalizers

#### Finalizers:

* **`kubernetes.io/service-account-token`** (actually for ServiceAccounts)
* **`service.kubernetes.io/load-balancer-cleanup`** (cloud provider)

Cloud-provider controllers add finalizers to **LoadBalancer Services**.

#### Scenario:

When you delete a `Service` of type `LoadBalancer`:

* It must delete the cloud load balancer (ELB, ALB, NLB, GCE LB).
* If deletion is async, the Service should not vanish until cleanup is safely done.

#### Example for AWS:

*   Finalizer might look like:

    ```
    service.kubernetes.io/aws-load-balancer-controller
    ```

#### How handled:

1. `kubectl delete svc mylb`
2. Service gets stuck in **Terminating**.
3. AWS load balancer controller deletes:
   * ELB resources
   * Security groups
   * Target groups
4. When done, controller removes finalizer → Service fully deletes.

***

## ✅ 6. ServiceAccount Finalizers

#### Finalizers:

* **`kubernetes.io/service-account-token`**

#### Scenario:

When a ServiceAccount is removed, Kubernetes must remove **all auto-generated tokens/secrets** associated with it.

#### How handled:

* Token controller deletes associated Secrets
* After cleanup, finalizer is removed

***

## ✅ 7. CustomResourceDefinitions (CRDs) Finalizers

#### Finalizers:

* **`custom-resource-definition-finalizer`**
* **`apiextensions.k8s.io/finalizer`**

#### Scenario:

CRDs cannot be deleted until **all instances of that CRD (CRs)** are removed.

Imagine you delete a CRD while 500 CRs still exist → that would leave orphaned entries in etcd.

#### How handled:

* Kubernetes waits for all CRs to be deleted
* Only then removes CRD finalizer

***

## ✅ 8. NetworkPolicy/Ingress Finalizers (Cloud provider or addon)

Calico, Cilium, AWS ALB Ingress Controller, GKE Ingress Controller, etc add finalizers such as:

* `finalizer.projectcalico.org`
* `ingress.k8s.aws/resources`
* `gke.ingress.finalizer`

#### Scenario:

Ensure cleanup of:

* Ingress load balancers
* Firewall rules / security groups
* ENIs
* BGP routes
* IP allocations

#### Example for AWS ALB:

Deleting an Ingress must first delete:

* ALB
* Target Groups
* Listeners
* SG rules

Only after that finalizer is removed.

***

## ✅ 9. VolumeSnapshot / CSI Drivers Finalizers

CSI Snapshot controllers commonly add:

* `snapshot.storage.kubernetes.io/controller-finalizer`
* `snapshot.storage.kubernetes.io/create-snapshot`

#### Scenario:

Ensure underlying cloud storage snapshots are deleted safely.

#### How handled:

1. User deletes VolumeSnapshot.
2. Snapshot controller talks to the CSI driver, deletes cloud snapshot.
3. Then removes finalizer → K8s deletes the object.

***

## ✅ 10. Jobs / Pods Finalizers (special case)

Pods rarely have finalizers except in special operators:

#### Example:

* **`kubernetes.io/pod-preemption`**
* **`batch.kubernetes.io/job-tracking`**

Jobs:

* Finalizers ensure Pod tracking and cleanup logic.

***

## ✅ 11. Network Interface / IPAM Controllers

Examples:

* `whereabouts.network.k8s.io`
* `k8s.cni.cncf.io/networks`

#### Scenario:

Ensure cleanup of allocated IPs or ENIs before object deletion.

***

## ✅ Quick Summary Table

| Resource Type  | Finalizer                 | Why it Exists                         |
| -------------- | ------------------------- | ------------------------------------- |
| PV             | `pv-protection`           | prevent deletion while bound          |
| PVC            | `pvc-protection`          | prevent deletion while in use by Pods |
| Namespace      | `kubernetes`              | delete all child resources safely     |
| Service (LB)   | provider finalizers       | delete cloud LB infra                 |
| ServiceAccount | `service-account-token`   | cleanup auto-generated tokens         |
| CRD            | `crd-finalizer`           | ensure all CRs are deleted first      |
| Ingress        | cloud/provider finalizers | cleanup cloud LB + firewall rules     |
| VolumeSnapshot | snapshot finalizers       | cleanup CSI snapshots                 |
| Calico/Cilium  | network finalizers        | cleanup BGP/IP allocations            |
| Pods/Jobs      | job tracking finalizers   | ensure cleanup logic                  |

***

