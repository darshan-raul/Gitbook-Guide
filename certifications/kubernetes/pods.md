# Pods

_Pods_ are the smallest deployable units of computing that you can create and manage in Kubernetes.

* a group of one or more [containers](https://kubernetes.io/docs/concepts/containers/), with **shared storage and network resources**
* a specification for how to run the containers.
* As well as application containers, a Pod can contain [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) that run during Pod startup.&#x20;
* You can also inject [ephemeral containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) for debugging if your cluster offers this

> Note: Pods are generally not created directly and are created using workload resources.

### Lifecycle

* Pods follow a defined lifecycle, starting in the `Pending` [phase](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-phase), moving through `Running` if at least one of its primary containers starts OK, and then through either the `Succeeded` or `Failed` phases depending on whether any container in the Pod terminated in failure.
* Pods are only [scheduled](https://kubernetes.io/docs/concepts/scheduling-eviction/) once in their lifetime. Once a Pod is scheduled (assigned) to a Node, the Pod runs on that Node until it stops or is [terminated](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-termination).

#### Types of probe <a href="#types-of-probe" id="types-of-probe"></a>

The kubelet can optionally perform and react to three kinds of probes on running containers:

* `livenessProbe`Indicates whether the container is running. If the liveness probe fails, the kubelet kills the container, and the container is subjected to its [restart policy](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy). If a container does not provide a liveness probe, the default state is `Success`.
* `readinessProbe`Indicates whether the container is ready to respond to requests. If the readiness probe fails, the endpoints controller removes the Pod's IP address from the endpoints of all Services that match the Pod. The default state of readiness before the initial delay is `Failure`. If a container does not provide a readiness probe, the default state is `Success`.
* `startupProbe`Indicates whether the application within the container is started. All other probes are disabled if a startup probe is provided, until it succeeds. If the startup probe fails, the kubelet kills the container, and the container is subjected to its [restart policy](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy). If a container does not provide a startup probe, the default state is `Success`.

> By default, all deletes are graceful within 30 seconds

#### Garbage collection of failed Pods <a href="#pod-garbage-collection" id="pod-garbage-collection"></a>

For failed Pods, the API objects remain in the cluster's API until a human or [controller](https://kubernetes.io/docs/concepts/architecture/controller/) process explicitly removes them.
