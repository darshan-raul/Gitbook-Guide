# Hypervisors

* **Level 0 (L0):** This is the base level, representing the physical hardware (the actual server) and the hypervisor that runs directly on it. In cloud environments like Google Cloud or AWS, the cloud provider manages this layer. This L0 hypervisor is responsible for virtualizing the physical hardware and presenting it to the next layer up.  &#x20;
* **Level 1 (L1) Virtualization:** This refers to the virtualization layer that runs _on top of_ the Level 0 hypervisor. The Level 1 entity is a virtual machine (VM) that the L0 hypervisor perceives as a guest. Within this L1 VM, a second hypervisor is installed and runs. This L1 hypervisor has access to the hardware-assisted virtualization features exposed by the L0 hypervisor (like Intel VT-x or AMD-V). The operating system and applications running directly on this L1 VM are considered to be at Level 1.
* **Level 2 (L2) Virtualization:** This is the subsequent layer of virtualization, where virtual machines (L2 VMs) are created and managed by the Level 1 hypervisor running inside the L1 VM. These L2 VMs are guests to the L1 hypervisor. From the perspective of the L1 VM, the L2 VMs are simply workloads it is running.  &#x20;

**Key Differences:**

The primary differences between Level 1 and Level 2 virtualization lie in their position within the virtualization stack and their direct relationship with the underlying hardware:

| **Feature**         | **Level 1 Virtualization**                                         | **Level 2 Virtualization**                                                                               |
| ------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| **Position**        | Runs directly on the Level 0 hypervisor.                           | Runs on the Level 1 hypervisor.                                                                          |
| **Hypervisor**      | Contains the Level 1 hypervisor.                                   | Contains the operating system and applications.                                                          |
| **Hardware Access** | Interacts with hardware features virtualized by the L0 hypervisor. | Accesses virtualized hardware provided by the L1 hypervisor (which in turn relies on the L0 hypervisor). |
| **Perceived Host**  | The physical hardware (virtualized by L0).                         | The Level 1 VM (acting as a host).                                                                       |



In essence, Level 1 virtualization is the initial layer of abstraction provided by the cloud provider's hypervisor, while Level 2 virtualization is the layer you create by installing and running your own hypervisor within that Level 1 VM. This nested structure allows for scenarios like running a different hypervisor within your cloud instance or creating complex virtualized environments for testing, development, or specific application requirements.
