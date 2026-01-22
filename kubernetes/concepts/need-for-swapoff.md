# need for Swapoff

The requirement to run `swapoff -a` on Kubernetes nodes has been a staple of cluster administration for years. However, as of 2026, the answer has changed from a hard "yes" to a "usually, but it's now optional."

Historically, Kubernetes required swap to be disabled because the system was designed for predictable performance. If a node starts "swapping" memory to disk, performance drops by orders of magnitude, and the Kubernetes scheduler (which doesn't account for swap) becomes unable to manage resources accurately.

***

#### Why was `swapoff` originally required?

The original "no-swap" policy was driven by four main technical hurdles:

1. QoS & Resource Guarantees: Kubernetes uses "Quality of Service" (QoS) classes (Guaranteed, Burstable, BestEffort). If swap is active, the Linux kernel might swap out a "Guaranteed" pod's memory to disk, violating the promise of instant RAM access.
2. Kubelet Eviction Logic: The `kubelet` monitors RAM to decide when to kill pods to save the node. Swap "hides" memory pressure from the kubelet, causing the node to become sluggish and unresponsive (disk thrashing) instead of cleanly evicting pods.
3. Scheduler Blindness: The Kubernetes Scheduler only tracks physical RAM.<sup>1</sup> If nodes have 16GB RAM + 16GB Swap, the scheduler still thinks there is only 16GB available. This leads to massive overcommitment and node instability.
4. Performance Unpredictability: Disk I/O is significantly slower than RAM. In a multi-tenant cluster, one "noisy neighbor" pod using swap can saturate the disk I/O, slowing down every other pod on that node.

***

#### Is it still needed in 2026?

No, it is no longer strictly required, but it is still the default.

Starting with Kubernetes v1.34/v1.35, swap support has graduated to Stable (GA) on Linux nodes. You can now run Kubernetes with swap enabled, provided your nodes use cgroup v2.<sup>2</sup>

#### How Swap works in modern Kubernetes

If you choose to leave swap on, you must explicitly configure the `kubelet` to handle it. You have two primary configuration modes:

| **Mode**         | **Behavior**                                                                             | **Best For**                                                                     |
| ---------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| NoSwap (Default) | Kubelet starts even if swap is on, but pods cannot use it. Only system daemons use swap. | Protecting node stability while keeping workloads on RAM.                        |
| LimitedSwap      | Pods can use swap, but it is limited. A pod's `memory limit` includes both RAM and Swap. | Workloads with large memory footprints but low "active" usage (e.g., Java apps). |

> Note: Even with `LimitedSwap`, a pod cannot exceed its defined memory limit. Swap simply allows the kernel to move "cold" (unused) memory pages to disk to free up actual RAM for active processes.

***

#### Should you still disable it?

For most production environments, disabling swap is still the best practice.<sup>3</sup> \* Turn it OFF if: You run latency-sensitive microservices, databases, or real-time APIs. Predictability is your priority.

* Turn it ON if: You are running memory-intensive workloads (like AI/ML models or large Java monoliths) where some parts of the memory are rarely accessed, or if you are running on resource-constrained edge hardware.

#### How to enable it (if you want to)

If you decide to leave swap on, you must update your `KubeletConfiguration`:

YAML

```
# /var/lib/kubelet/config.yaml
failSwapOn: false
memorySwap:
  swapBehavior: LimitedSwap
```

