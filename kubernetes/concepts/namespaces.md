# Namespaces

{% embed url="https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/" %}

* provide a mechanism for isolating groups of resources within a single cluster.
* Names of resources need to be unique within a namespace, but not across namespaces.
* **Namespace-based scoping** is applicable only for namespaced [objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/#kubernetes-objects) _(e.g. Deployments, Services, etc.)_ and <mark style="background-color:purple;">not for cluster-wide objects</mark> <mark style="background-color:purple;"></mark>_<mark style="background-color:purple;">(e.g. StorageClass, Nodes, PersistentVolumes, etc.)</mark>_<mark style="background-color:purple;">.</mark>

> <mark style="background-color:orange;">For a production cluster, consider</mark> <mark style="background-color:orange;"></mark>_<mark style="background-color:orange;">not</mark>_ <mark style="background-color:orange;"></mark><mark style="background-color:orange;">using the</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">`default`</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">namespace</mark>. Instead, make other namespaces and use those.



### Gotchas

* Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. Start using namespaces when you need the features they provide.
* It is **not necessary to use multiple namespaces to separate slightly different resources**, such as different versions of the same software: use [labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels) to distinguish resources within the same namespace.
* Avoid creating namespaces with the prefix `kube-`, since it is reserved for Kubernetes system namespaces.\


\
