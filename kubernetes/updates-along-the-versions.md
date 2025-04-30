# updates along the versions

{% embed url="https://www.youtube.com/playlist?list=PL2We04F3Y_42ay3zqo1XHXWpG1RMdDpBM" %}

**Kubernetes v1.19 (Released: August 2020)**

_Compared to v1.18:_

* **Extended Support:** Starting with v1.19, the official support window was extended to one year.
* <mark style="background-color:green;">**EndpointSlice API (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA,</mark> providing a scalable and extensible alternative to Endpoints.
* **I**<mark style="background-color:green;">**ngress API (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">The</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">`networking.k8s.io/v1`</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Ingress API graduated to GA.</mark> The `IngressClass` resource was introduced as a replacement for the `kubernetes.io/ingress.class` annotation.
* **seccomp Support (Alpha):** Added a `seccompProfile` field to Pod `securityContext` and container `securityContext` for configuring seccomp profiles.
* **Immutable Secrets and ConfigMaps (Beta):** Allowed marking Secrets and ConfigMaps as immutable to prevent accidental updates.
* **Generic Ephemeral Volumes (Alpha):** Introduced a way to provision ephemeral volumes using standard storage drivers that support PersistentVolumes.
* **CertificateSigningRequest API (Beta):** Improvements made to the CSR API and approval process.
* **Kubelet CRI Support:** Introduced support for Container Runtime Interface (CRI) v1.
* **Structured Logging:** Continued effort towards structured logging across components.

***

**Kubernetes v1.20 (Released: December 2020)**

_Compared to v1.19:_

* **Volume Snapshot Operations (Stable):** Graduated to GA, allowing standard operations for volume snapshots.
* **kubectl debug (Beta):** The `kubectl debug` feature moved to Beta, improving debugging capabilities for pods and nodes.
* **API Priority and Fairness (Beta):** Enabled by default, helping prevent API server overloading by classifying and prioritizing requests.
* **PID Limits (Stable):** Graduated to GA, allowing limits on process IDs within pods to improve node stability.
* **Graceful Node Shutdown (Alpha):** Introduced kubelet awareness of node shutdowns for graceful pod termination.
* **CronJob Controller v2 (Alpha):** Introduced an optional, more performant CronJob controller implementation.
* **IPv4/IPv6 Dual-Stack Support (Beta):** Re-implemented dual-stack support, enabling Services to operate in dual-stack mode.
* <mark style="background-color:red;">**Dockershim Deprecation:**</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">Announced the future removal of the built-in Docker runtime support (Dockershim) in favor of CRI-compliant runtimes</mark>.
* **Exec Probe Timeout Handling:** Addressed inconsistencies where kubelet did not always respect exec probe timeouts.
* <mark style="background-color:green;">**RuntimeClass (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA.</mark>
* **RootCAConfigMap:** Introduced `RootCAConfigMap` for distributing cluster CA bundles to pods.

***

**Kubernetes v1.21 (Released: April 2021)**

_Compared to v1.20:_

* **CronJob (Stable):** The CronJob API (`batch/v1`) graduated to GA<mark style="background-color:green;">.</mark>
* <mark style="background-color:green;">**Immutable Secrets and ConfigMaps (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA.</mark>
* **Graceful Node Shutdown (Beta):** Moved to Beta, allowing configuration via Kubelet config.
* **PersistentVolume Health Monitor (Beta):** Introduced monitoring for PV health from CSI drivers.
* <mark style="background-color:green;">**PodDisruptionBudget API (**</mark><mark style="background-color:green;">**`policy/v1`**</mark><mark style="background-color:green;">**) (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA</mark>. The `policy/v1beta1` version was deprecated. An empty selector (`{}`) now selects all pods in the namespace.
* **IPv6 Dual-Stack Support (Beta - Enabled by Default):** Made significant progress towards stable, enabling by default.
* <mark style="background-color:red;">**PodSecurityPolicy Deprecation:**</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">PSP was officially deprecated and scheduled for removal in v1.25</mark>. Pod Security Admission was introduced as the intended replacement.
* **Topology Aware Hints (Alpha):** Introduced as a successor to the deprecated `topologyKeys` field for topology-aware routing.
* **Indexed Job (Alpha):** Added support for Jobs managing pods associated with completion indexes (e.g., for parallel processing).
* **Suspend Job (Alpha):** Added the ability to suspend and resume Jobs.

***

**Kubernetes v1.22 (Released: August 2021)**

_Compared to v1.21:_

* **Major API Removals:** <mark style="background-color:red;">Many deprecated beta APIs were removed, requiring migration to their stable</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">`v1`</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">counterparts</mark>. This included:
  * `admissionregistration.k8s.io/v1beta1` (Validating/MutatingWebhookConfiguration)
  * `apiextensions.k8s.io/v1beta1` (CustomResourceDefinition)
  * `apiregistration.k8s.io/v1beta1` (APIService)
  * `authentication.k8s.io/v1beta1` (TokenReview)
  * `authorization.k8s.io/v1beta1` (SubjectAccessReview <sup>1</sup> types)   [1. github.com](https://github.com/kyverno/policies/issues/80)[github.com](https://github.com/kyverno/policies/issues/80)
  * `certificates.k8s.io/v1beta1` (CertificateSigningRequest)
  * `coordination.k8s.io/v1beta1` (Lease)
  * `extensions/v1beta1` and `networking.k8s.io/v1beta1` (Ingress - `networking.k8s.io/v1` is stable)
  * RBAC resources (`rbac.authorization.k8s.io/v1beta1`)
  * Storage resources (`storage.k8s.io/v1beta1`)
* <mark style="background-color:green;">**Server-Side Apply (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA</mark>, providing a declarative way to manage resource configurations.
* **Bound Service Account Tokens (Beta):** Improved security for service account tokens by binding them to specific pods.
* **Windows Privileged Containers (Alpha):** Introduced HostProcess containers for running privileged operations on Windows nodes.
* <mark style="background-color:green;">**Swap on Linux (Alpha):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Added preliminary support for allowing Kubernetes nodes to use swap memory.</mark>
* **Default Seccomp Profiles (Alpha):** Introduced the ability to apply a default seccomp profile (`RuntimeDefault`) cluster-wide.
* **Memory QoS with cgroups v2 (Alpha):** Enabled memory quality-of-service features using Linux cgroup v2.
* **etcd Updated:** Upgraded to etcd v3.5.0.

***

**Kubernetes v1.23 (Released: December 2021)**

_Compared to v1.22:_

* <mark style="background-color:green;">**IPv4/IPv6 Dual-Stack Networking (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA.</mark>
* **HorizontalPodAutoscaler `v2` API (Stable):** The `autoscaling/v2` API graduated to GA, replacing `v2beta2`.
* **Generic Ephemeral Volumes (Stable):** Graduated to GA.
* **Skip Volume Ownership Change (Stable):** The `fsGroupChangePolicy` field graduated to GA, allowing faster pod startup by skipping recursive permission changes.
* **Pod Security Admission (Beta - Enabled by Default):** The replacement for PSP was enabled by default, providing built-in pod security standards (Privileged, Baseline, Restricted).
* **Ephemeral Containers (Beta - Enabled by Default):** `kubectl debug` functionality using ephemeral containers was enabled by default.
* **CRD Validation Expression Language (Beta):** Introduced CEL for defining custom validation rules directly within CRDs.
* **gRPC Probes (Alpha):** Added support for gRPC health probes for pods.
* **Structured Logging (Beta):** Made significant progress adopting structured logging across components.
* **Job Tracking without Pod Completion (Alpha):** Added tracking based on Job controller status rather than just Pod status.
* **OpenAPI v3 Support (Beta):** Improved support for publishing Kubernetes APIs via OpenAPI v3.

***

**Kubernetes v1.24 (Released: May 2022)**

_Compared to v1.23:_

* _<mark style="background-color:red;">**Dockershim Removed: The most significant change. The built-in Docker Engine support via dockershim was completely removed. Clusters must now use other CRI-compliant runtimes like containerd or CRI-O.**</mark>_
* <mark style="background-color:green;">**New Image Registry:**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">The official container image registry moved from</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">`k8s.gcr.io`</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">to</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">`registry.k8s.io`</mark><mark style="background-color:green;">.</mark>
* **Service Account Token Secrets:** Automatic generation of secret-based service account tokens was disabled by default (`LegacyServiceAccountTokenNoAutoGeneration=true`). The TokenRequest API should be used instead.
* **Beta APIs Off by Default:** New beta APIs are no longer enabled by default in clusters. They must be explicitly enabled. Existing beta APIs remained enabled.
* **Storage Capacity Tracking (Stable):** Graduated to GA.
* **Volume Expansion for StatefulSets (Stable):** Graduated to GA.
* **Non-Graceful Node Shutdown Handling (Beta):** Moved to Beta.
* **Contextual Logging (Alpha):** Introduced concept for logging with contextual information.
* **OpenAPI v3 (Stable):** Kubernetes API publishing via OpenAPI v3 graduated to GA.
* **Network Policy Status (Alpha):** Added a status subresource to NetworkPolicy.

***

**Kubernetes v1.25 (Released: August 2022)**

_Compared to v1.24:_

* <mark style="background-color:red;">**PodSecurityPolicy (PSP) Removed:**</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">The deprecated PSP API was completely removed. Users must migrate to Pod Security Admission (PSA) or third-party alternatives.</mark>
* <mark style="background-color:green;">**Ephemeral Containers (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA.</mark>
* <mark style="background-color:green;">**cgroups v2 Support (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Support for the Linux cgroup v2 API graduated to GA.</mark>
* **Core CSI Migration (Stable):** The framework for migrating in-tree volume plugins (like AWS EBS, GCE PD, Azure Disk) to out-of-tree CSI drivers graduated to GA. Specific driver migrations continued.
* **Windows HostProcess Containers (Beta):** Moved to Beta.
* **`SeccompDefault` (Beta):** Enabling default seccomp profiles cluster-wide moved to Beta.
* **`endPort` in NetworkPolicy (Stable):** Support for specifying port ranges in Network Policies graduated to GA.
* **Local Ephemeral Storage Capacity Isolation (Stable):** Graduated to GA.
* **KMS v2 API (Alpha):** Introduced a new Key Management Service API (`v2alpha1`) for encryption at rest.
* **API Removals:** Several deprecated beta APIs were removed, including `batch/v1beta1` (CronJob) and `policy/v1beta1` (PodDisruptionBudget, PodSecurityPolicy).
* **In-tree Plugin Deprecations/Removals:** GlusterFS & Portworx plugins deprecated; Flocker, Quobyte, StorageOS removed.

***

**Kubernetes v1.26 (Released: December 2022)**

_Compared to v1.25:_

* **Major API Removals:** More deprecated beta APIs removed, including:
  * `flowcontrol.apiserver.k8s.io/v1beta1` (API Priority and Fairness)
  * `autoscaling/v2beta2` (HorizontalPodAutoscaler)
* **CRI `v1alpha2` Removed:** Support for the older CRI `v1alpha2` runtime API was removed. Runtimes must implement CRI `v1` (e.g., containerd >= 1.6.0).
* **CPUManager Policy Options (Beta):** New options for the CPU Manager graduated to Beta. CPU Manager itself graduated to Stable.
* **Aggregated Discovery (Alpha):** Introduced a way to reduce API server discovery load for clients.
* **Dynamic Resource Allocation (Alpha):** Introduced a new API for requesting specialized hardware resources (e.g., FPGAs, GPUs) beyond CPU/memory.
* **Delegating FSGroup to CSI Driver (Stable):** Graduated to GA. Allows CSI drivers to handle `fsGroup` permissions.
* **Pod Scheduling Readiness (Alpha):** Introduced `schedulingGates` to allow external controllers to influence when a pod is considered ready for scheduling.
* **Non-Graceful Node Shutdown Handling (Beta - Enabled by Default):** Feature became enabled by default.
* **Cross-Namespace Volume Data Sources (Alpha):** Allowed specifying PVC data sources from a different namespace.
* **Retroactive Default StorageClass Assignment (Beta):** Allowed assigning a default StorageClass to existing unbound PVCs.
* **CEL for Admission Control (Beta):** Using Common Expression Language (CEL) for admission control rules moved to Beta.
* **KMS v2 Improvements (Beta):** Enhanced the v2 KMS API.

***

**Kubernetes v1.27 (Released: April 2023)**

_Compared to v1.26:_

* <mark style="background-color:green;">**`SeccompDefault`**</mark><mark style="background-color:green;">**&#x20;**</mark><mark style="background-color:green;">**(Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA, allowing secure-by-default seccomp profiles.</mark>
* **Mutable Scheduling Directives for Jobs (Alpha):** Allowed updating scheduling directives (like `suspend`) for Jobs after creation.
* **ReadWriteOncePod PV Access Mode (Beta):** A PV access mode restricting volume access to a single pod at a time moved to Beta.
* **Pod Scheduling Readiness (Beta):** Moved to Beta.
* **Node Log Query (`kubectl node-logs`) (Alpha):** Added a feature gate and `kubectl` plugin to query logs directly from nodes via the Kubelet API.
* **Fine-Grained Pod Topology Spread Policies (Beta):** Features like `minDomains`, `nodeAffinityPolicy`, and `nodeTaintsPolicy` for Pod Topology Spread constraints moved to Beta.
* <mark style="background-color:green;">**In-Place Pod Vertical Scaling (Alpha):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Introduced an alpha feature gate allowing resizing of pod CPU/Memory requests and limits without restarting the pod.</mark>
* **Volume Expansion Features (Stable):** `ExpandCSIVolumes`, `ExpandInUsePersistentVolumes`, `ExpandPersistentVolumes` feature gates graduated to GA and were locked on.
* **Feature Gate Removals:** Several feature gates were removed as their features graduated (e.g., `CSIMigration`, `CSIInlineVolume`, `EphemeralContainers`).
* **Kubelet Evented PLEG (Alpha):** Introduced for potential performance improvements in pod lifecycle event generation.

***

**Kubernetes v1.28 (Released: August 2023)**

_Compared to v1.27:_

* **Sidecar Containers (Beta):** Introduced native support for sidecar container patterns using restartable init containers.
* <mark style="background-color:green;">**Non-Graceful Node Shutdown Handling (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA.</mark>
* **Recovery from Non-Graceful Shutdown (Alpha):** Added capability for stateful workloads to recover faster after node shutdown.
* **CRD Validation Rules Language (Stable):** Graduated to GA.
* **Admission Control with CEL (Stable):** Graduated to GA.
* <mark style="background-color:green;">**KMS v2 Encryption Provider (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">The</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">`v2`</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">KMS API graduated to GA.</mark>
* **Job Mutable Scheduling Directives (Beta):** Moved to Beta.
* **API awareness of load balancer behavior (Alpha):** Added features for better integration between Services (LoadBalancer type) and cloud provider load balancers.
* **Expanded DNS Configuration (Alpha):** Allowed more control over Pod DNS settings.
* **`kubectl apply --prune` Alpha feature removal.**

***

**Kubernetes v1.29 (Released: December 2023)**

_Compared to v1.28:_

* <mark style="background-color:green;">**ReadWriteOncePod PV Access Mode (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA.</mark>
* **Node Volume Expansion Secret Support (Stable):** CSI drivers can now use secrets during node-side volume expansion; graduated to GA.
* **Init Containers Restart Policy (Beta):** Init containers gained a `restartPolicy` field (default `Always`), allowing them to restart until completion, useful for sidecar patterns.
* **Ensure Secret Pulled Images (Beta):** Kubelet can be configured to always pull images even if present locally when using imagePullSecrets.
* **Queueing Hint Functions for Pod Scheduling (Beta):** Allows scheduler plugins to provide hints on when to retry scheduling unschedulable pods.
* **KMS v2 Improvements (Stable):** Continued stabilization of KMS v2 features.
* **Reduction of Secret-based Service Account Tokens (Beta):** Progressed towards removing auto-generated secret tokens.
* **Structured Authorization Configuration (Alpha):** Introduced a structured config file for configuring authorization webhooks.
* **Pod User Namespaces Support (Alpha):** Introduced support for running pods within isolated user namespaces for enhanced security.
* **Cluster Trust Bundles (Alpha):** Provided a mechanism to distribute X.509 certificate bundles within the cluster.
* <mark style="background-color:red;">**Clean-up of Legacy Package Repositories:**</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">Moved away from Google-hosted package repositories (</mark><mark style="background-color:red;">`apt.kubernetes.io`</mark><mark style="background-color:red;">,</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">`yum.kubernetes.io`</mark><mark style="background-color:red;">).</mark>

***

**Kubernetes v1.30 (Released: April 2024)**

_Compared to v1.29:_

* **Pod User Namespaces (Beta):** Moved to Beta.
* <mark style="background-color:green;">**AppArmor Support (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Graduated to GA, allowing enforcement of AppArmor profiles.</mark>
* **Node Log Query (Beta):** Moved to Beta.
* **Structured Authorization Configuration (Beta):** Moved to Beta.
* **Reduction of Secret-based Service Account Tokens (Stable):** Graduated to GA; auto-generation is off by default and discouraged.
* **Bound Service Account Token Improvements (Beta):** Enhanced security and functionality of projected service account tokens.
* **Prevent Unauthorized Volume Mode Conversion (Beta):** Added checks to prevent malicious users from changing volume modes.
* **Volume Attributes Class (Alpha):** Introduced a way to define storage classes based on volume attributes.
* **PodReadyToStartContainers Condition (Alpha):** Added a Pod condition indicating readiness for container startup.
* **Contextual Logging (Stable):** Graduated to GA.
* **`kubectl delete -i` (interactive mode) (Beta):** Moved to Beta.
* **Go Version:** Updated to Go 1.22.

***

**Kubernetes v1.31 (Released: August 2024)**

_Compared to v1.30:_

* <mark style="background-color:green;">**Containerd Shims (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Management of containerd shims graduated to GA, simplifying runtime plugin management.</mark>
* **MaxUnavailable for StatefulSets (Beta):** Introduced `maxUnavailable` for StatefulSet rolling updates, similar to Deployments.
* **Pod Lifecycle Sleep Action (Alpha):** Added an alpha feature to inject sleep delays during container startup/shutdown for debugging or coordination.
* **MatchLabelKeys/MismatchLabelKeys for Pod Topology (Beta):** Enhanced Pod Affinity/Anti-Affinity rules for more dynamic matching based on label keys.
* **Admission Control CEL Improvements:** Added more functions, metrics, and testing capabilities for CEL-based admission control.
* **Structured Authentication Config (Stable):** Graduated to GA.
* **Validating Admission Policy Type Checking (Alpha):** Improved static analysis for admission policies.
* **Kubelet Tracing (Alpha):** Added experimental tracing support to the kubelet.
* **API Server Self-Signed Cert Rotation (Alpha):** Introduced automatic rotation for self-signed certificates used internally by the API server.
* **Go Version:** Updated Go toolchain (likely 1.22.x patch).

***

**Kubernetes v1.32 (Released: December 2024)**

_Compared to v1.31:_

* **API Server Tracing (Stable):** Graduated to GA.
* <mark style="background-color:green;">**Sidecar Containers (Stable):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Native sidecar container support graduated to GA.</mark>
* **Recursive Read-only (RRO) Mounts (Beta):** Allows mounting volumes and their submounts recursively as read-only.
* <mark style="background-color:green;">**In-Place Pod Vertical Scaling (Beta):**</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">Moved to Beta, allowing CPU/Memory resizing without pod restart.</mark>
* **Pod User Namespaces (Beta - Host Network Support):** Added support for host network pods within user namespaces.
* **Kubelet Resource Metrics Endpoint (Stable):** The `/metrics/resource` endpoint graduated to GA.
* **Prevent Unauthorized Volume Mode Conversion (Stable):** Graduated to GA.
* **Validating Admission Policy Improvements:** Continued stabilization and feature additions.
* **API Removals:** `flowcontrol.apiserver.k8s.io/v1beta2` removed (use `v1beta3`). `autoscaling/v2beta2` HorizontalPodAutoscaler removed (use `v2`).
* **Go Version:** Updated to Go 1.23.x.
