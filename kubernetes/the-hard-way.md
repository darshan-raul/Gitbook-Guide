# The hard way

Then best way to be crystal clear about all of the internal components of k8s is to go through this excellent repo on github, its a derivative of the infamous k8s the hard way by kelsey hightower, but this one is much harder. Tests you but is very comprehensive and you will learn a lot

{% embed url="https://github.com/ghik/kubernetes-the-harder-way/tree/linux" %}

You start by creating 7 vms using QEMU \[note to enable nested virtualization if working on a cloud vm]

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Then created a 3 etcd cluster

<figure><img src="../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

managed to create  a kube api cluster fronted by a ipvsadm/ldirectory loadbalancing setup working with weighted round robin

<figure><img src="../.gitbook/assets/image (273).png" alt=""><figcaption></figcaption></figure>

installed kubeapi server and cni,containerd and kubelet on all servers

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>
