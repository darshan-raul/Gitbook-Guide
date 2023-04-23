# Observability

The [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) uses <mark style="background-color:orange;">**liveness probes**</mark> to know when to restart a container. For example, liveness probes could catch a deadlock, where an application is running, but unable to make progress. Restarting a container in such a state can help to make the application more available despite bugs.

The kubelet uses <mark style="background-color:orange;">**readiness probes**</mark> to know when a container is ready to start accepting traffic. A Pod is considered ready when all of its containers are ready. One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.

The kubelet uses <mark style="background-color:orange;">**startup probes**</mark> to know when a container application has started. If such a probe is configured, it disables liveness and readiness checks until it succeeds, making sure those probes don't interfere with the application startup. This can be used to adopt liveness checks on slow starting containers, avoiding them getting killed by the kubelet before they are up and running.

\


[Probes](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#probe-v1-core) have a number of fields that you can use to more precisely control the behavior of liveness and readiness checks:

* `initialDelaySeconds`: Number of seconds after the container has started before liveness or readiness probes are initiated. Defaults to 0 seconds. Minimum value is 0.
* `periodSeconds`: How often (in seconds) to perform the probe. Default to 10 seconds. Minimum value is 1.
* `timeoutSeconds`: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.
* `successThreshold`: Minimum consecutive successes for the probe to be considered successful after having failed. Defaults to 1. Must be 1 for liveness and startup Probes. Minimum value is 1.
* `failureThreshold`: When a probe fails, Kubernetes will try `failureThreshold` times before giving up. Giving up in case of liveness probe means restarting the container. In case of readiness probe the Pod will be marked Unready. Defaults to 3. Minimum value is 1.
