# cgroup v1 vs v2

The shift from cgroup v1 to v2 is one of the most significant changes in the Linux kernel for container orchestration.<sup>1</sup> While v1 was a collection of independent controllers, v2 is a redesigned, unified system that allows Kubernetes to manage resources with much higher precision.

#### The Core Architectural Shift

The most fundamental difference is how the "hierarchy" (the tree structure) is organized.<sup>2</sup>

* cgroup v1 (Multi-Hierarchy): Every resource (CPU, Memory, I/O) has its own separate tree. A process might be in `/sys/fs/cgroup/cpu/pod1` and `/sys/fs/cgroup/memory/pod1`. These trees don't talk to each other, making it nearly impossible to account for resources that overlap (like a process using high I/O which then triggers high CPU).
* cgroup v2 (Unified Hierarchy): There is only one tree.<sup>3</sup> If a Pod is in a cgroup, all its resource limits (CPU, RAM, I/O, PID) are managed in that single folder. This allows the kernel to see the "whole picture" of a container's resource footprint.

***

#### Key Differences at a Glance

| **Feature**       | **cgroup v1**                         | **cgroup v2**                             |
| ----------------- | ------------------------------------- | ----------------------------------------- |
| Hierarchy         | Multiple (one per controller)         | Single Unified Hierarchy                  |
| Process Placement | Can be anywhere in the tree           | Only at "Leaf" nodes (bottom of the tree) |
| Swap Support      | Poor/Unreliable in K8s                | Fully Supported (LimitedSwap)             |
| Monitoring        | Basic metrics                         | PSI (Pressure Stall Information)          |
| Security          | Difficult to delegate (Root required) | Safe "Rootless" delegation                |
| Controller Names  | `memory.limit_in_bytes`               | `memory.max` (Standardized)               |

***

#### Why Kubernetes Cares

For an SRE or cluster admin, the move to v2 unlocks three "killer features":

**1. Predictable Swap Management**

As discussed, v2 allows Kubernetes to distinguish between RAM and Swap usage accurately.<sup>4</sup> In v1, if a container hit its memory limit, the kernel might just start swapping it without the `kubelet` knowing, leading to a "zombie" pod that is technically alive but performing at 1% speed. In v2, the `memory.swap.max` control ensures swap is used exactly how you define it.<sup>5</sup>

**2. PSI (Pressure Stall Information)**<sup>**6**</sup>

This is a "game-changer" for monitoring. Instead of just seeing "CPU is at 90%", PSI tells you why the system is slow. It provides metrics on how long processes were stalled waiting for CPU, Memory, or I/O.

> Example: You can now see that a Pod isn't slow because of its own code, but because it's waiting 200ms for the disk to respond.

**3. The "No Internal Process" Rule**

In v1, you could have a process in a parent folder and child folders. This caused "resource competition" where the parent process stole from its own children. In v2, processes must live in leaf nodes.<sup>7</sup> This ensures that resource accounting is mathematically perfect—the sum of children's usage always equals the parent's reported usage.

***

#### How to Check Your Version

Since you are using Fedora and Ubuntu 24.04, you are almost certainly already on v2, as both distributions have defaulted to it for several years.

You can verify this in your terminal (works in both Bash and Nushell):

Bash

```
stat -fc %T /sys/fs/cgroup/
```

* Output `cgroup2fs`: You are on v2 (Modern).<sup>8</sup>
* Output `tmpfs`: You are on v1 (Legacy).<sup>9</sup>

####
