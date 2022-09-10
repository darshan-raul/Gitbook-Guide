# Windows containers

To enable the orchestration of Windows containers in Kubernetes, include Windows nodes in your existing Linux cluster. Scheduling Windows containers in [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) on Kubernetes is similar to scheduling Linux-based containers.

In order to run Windows containers, your Kubernetes cluster must include multiple operating systems.&#x20;

While you **can only run the** [**control plane**](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) **on Linux,** you can deploy worker nodes running either Windows or Linux.

Windows [nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) are [**supported**](https://kubernetes.io/docs/concepts/windows/intro/#windows-os-version-support) **provided that the operating system is Windows Server 2019.**

****

****
