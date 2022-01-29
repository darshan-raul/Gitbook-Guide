# Services and networking

###

**4 distinct networking problems to address:**

1. Highly-coupled container-to-container communications: this is solved by [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) and `localhost` communications.
2. Pod-to-Pod communications.
3. Pod-to-Service communications
4. External-to-Service communications



<mark style="background-color:orange;">**The Kubernetes networking model defines a set of fundamental rules:**</mark>

* **A pod in the cluster should be able to freely communicate with any other pod** without the use of Network Address Translation (NAT).
* **Any program running on a cluster node should communicate with any pod** on the same node without using NAT.
* **Each pod has its own IP address** (IP-per-Pod), and every other pod can reach it at that same address.

### Cluster networking

{% embed url="https://kubernetes.io/docs/concepts/cluster-administration/networking" %}

### Pod Networking

* Each Pod is assigned a unique IP address for each address family.

Every container in a Pod shares the network namespace, including the IP address and network ports. Inside a Pod (and **only** then), the containers that belong to the Pod can communicate with one another using `localhost`.&#x20;

The containers in a Pod can also communicate with each other using standard inter-process communications like SystemV semaphores or POSIX shared memory.&#x20;

Containers within the Pod see the system hostname as being the same as the configured `name` for the Pod.&#x20;

[https://learnk8s.io/kubernetes-network-packets](https://learnk8s.io/kubernetes-network-packets)

\


