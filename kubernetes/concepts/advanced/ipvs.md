# IPVS

IPVS (IP Virtual Server) is a Linux kernel feature designed for high-performance Layer 4 load balancing. It enables efficient distribution of network traffic across multiple backend servers by operating within the kernel space, thus reducing overhead and latency. ([IP Virtual Server](https://en.wikipedia.org/wiki/IP_Virtual_Server?utm_source=chatgpt.com))

#### What is IPVS?

IPVS is part of the Linux Virtual Server (LVS) project and provides transport-layer load balancing. It supports various scheduling algorithms, including: ([IP Virtual Server](https://en.wikipedia.org/wiki/IP_Virtual_Server?utm_source=chatgpt.com))

* **Round Robin (rr)**: Distributes requests evenly across all servers.
* **Weighted Round Robin (wrr)**: Distributes requests based on server weights.
* **Least Connection (lc)**: Sends requests to the server with the fewest active connections.
* **Source Hashing (sh)**: Routes requests based on the source IP address. ([Virtual IPs and Service Proxies | Kubernetes](https://kubernetes.io/docs/reference/networking/virtual-ips/?utm_source=chatgpt.com))

These algorithms allow for flexible and efficient traffic management, making IPVS suitable for large-scale deployments.

#### What is ipvsadm?

`ipvsadm` is a user-space utility for configuring and managing IPVS. It allows administrators to define virtual services and their associated real servers, set scheduling algorithms, and monitor the status of the load balancer. For example: ([K8s — kube-proxy Introduction](https://readmedium.com/k8s-kube-proxy-introduction-c847915efe57?utm_source=chatgpt.com), [IP Virtual Server](https://en.wikipedia.org/wiki/IP_Virtual_Server?utm_source=chatgpt.com))

```bash
ipvsadm -A -t 10.0.0.1:80 -s rr
ipvsadm -a -t 10.0.0.1:80 -r 192.168.1.2:80 -m
ipvsadm -a -t 10.0.0.1:80 -r 192.168.1.3:80 -m
```

These commands set up a virtual service at 10.0.0.1:80 using round-robin scheduling and add two real servers to handle the traffic. ([K8s — kube-proxy Introduction](https://readmedium.com/k8s-kube-proxy-introduction-c847915efe57?utm_source=chatgpt.com))

#### How Does Kubernetes Leverage IPVS?

In Kubernetes, `kube-proxy` is responsible for directing traffic to the appropriate backend pods. It supports multiple proxy modes: ([Advanced Networking with Kube-proxy in Kubernetes: A Practical Guide (with Examples) - Sling Academy](https://www.slingacademy.com/article/advanced-networking-with-kube-proxy-in-kubernetes/?utm_source=chatgpt.com))

* **iptables**: Uses iptables rules to manage traffic.
* **ipvs**: Utilizes IPVS for load balancing.
* **nftables**: Employs nftables for packet filtering and classification. ([Virtual IPs and Service Proxies | Kubernetes](https://kubernetes.io/docs/reference/networking/virtual-ips/?utm_source=chatgpt.com), [Comparing kube-proxy modes: iptables or IPVS?](https://www.tigera.io/blog/comparing-kube-proxy-modes-iptables-or-ipvs/?utm_source=chatgpt.com))

When running in IPVS mode, `kube-proxy` configures IPVS to handle service traffic, offering improved performance and scalability compared to iptables mode. This is particularly beneficial in clusters with a large number of services and endpoints, as IPVS uses hash tables for faster lookups, resulting in lower latency and higher throughput. ([Virtual IPs and Service Proxies | Kubernetes](https://kubernetes.io/docs/reference/networking/virtual-ips/?utm_source=chatgpt.com), [Running kube-proxy in IPVS Mode - Amazon EKS](https://docs.aws.amazon.com/eks/latest/best-practices/ipvs.html?utm_source=chatgpt.com))

To enable IPVS mode, ensure that the necessary kernel modules are loaded and start `kube-proxy` with the `--proxy-mode=ipvs` flag. Additionally, `ipvsadm` can be used to inspect and manage the IPVS configuration. ([Advanced Networking with Kube-proxy in Kubernetes: A Practical Guide (with Examples) - Sling Academy](https://www.slingacademy.com/article/advanced-networking-with-kube-proxy-in-kubernetes/?utm_source=chatgpt.com))

#### Current Status in Kubernetes

As of Kubernetes v1.31, a new proxy mode using `nftables` has been introduced. While IPVS mode remains supported, `nftables` offers similar or better performance with a more straightforward implementation. The Kubernetes project suggests that IPVS mode may be deprecated in the future, and users are encouraged to consider transitioning to `nftables` mode. ([NFTables mode for kube-proxy | Kubernetes](https://kubernetes.io/blog/2025/02/28/nftables-kube-proxy/?utm_source=chatgpt.com))

It's important to note that while IPVS provides advanced load balancing features, its complexity and maintenance overhead have led to a shift towards simpler and more maintainable solutions like `nftables`. ([Virtual IPs and Service Proxies | Kubernetes](https://kubernetes.io/docs/reference/networking/virtual-ips/?utm_source=chatgpt.com))

#### Alternatives to kube-proxy

Some Kubernetes deployments have adopted alternatives to `kube-proxy`, such as Cilium. Cilium leverages eBPF (extended Berkeley Packet Filter) to provide high-performance networking, load balancing, and security features without relying on traditional proxy modes. It offers a modern approach to Kubernetes networking and is gaining popularity in the community. ([Cilium (computing)](https://en.wikipedia.org/wiki/Cilium_\(computing\)?utm_source=chatgpt.com))

In summary, while IPVS and `ipvsadm` have played a significant role in Kubernetes networking, the ecosystem is evolving towards newer technologies like `nftables` and eBPF-based solutions for improved performance and maintainability.
