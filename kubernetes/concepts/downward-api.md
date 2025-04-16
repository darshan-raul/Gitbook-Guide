# Downward API

The Kubernetes Downward API allows containers to access metadata about themselves or the cluster without directly querying the Kubernetes API server. This metadata can be exposed to containers either as environment variables or through files mounted via a special volume type.

***

#### 🔍 Use Cases for the Downward API

The Downward API is particularly useful in scenarios such as:

* **Dynamic Configuration**: Injecting pod-specific information (like pod name or namespace) into applications that require such data for configuration or logging purposes.
* **Resource Awareness**: Providing applications with information about their own resource limits and requests, enabling them to adjust behavior accordingly.
* **Metadata-Driven Behavior**: Allowing applications to modify behavior based on labels or annotations assigned to the pod.

***

#### 🛠 Practical Example: Exposing Pod Metadata via Environment Variables

Here's a step-by-step guide to using the Downward API to expose pod metadata as environment variables:

1.  **Create a Pod Definition File**: Save the following YAML content into a file named `downward-api-env.yaml`:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: downward-api-example
    spec:
      containers:
      - name: main
        image: busybox
        command: ["sh", "-c", "env && sleep 3600"]
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
        - name: CPU_REQUEST
          valueFrom:
            resourceFieldRef:
              resource: requests.cpu
              divisor: 1m
        - name: MEMORY_LIMIT
          valueFrom:
            resourceFieldRef:
              resource: limits.memory
              divisor: 1Mi
    ```
2.  **Apply the Pod Configuration**: Deploy the pod to your Kubernetes cluster using the following command:

    ```bash
    kubectl apply -f downward-api-env.yaml
    ```
3.  **Verify the Environment Variables**: Once the pod is running, you can check the environment variables set within the container:

    ```bash
    kubectl exec downward-api-example -- env | grep -E 'POD_NAME|POD_NAMESPACE|POD_IP|NODE_NAME|CPU_REQUEST|MEMORY_LIMIT'
    ```

    This command will display the values of the specified environment variables, confirming that the Downward API has correctly injected the pod metadata.

***

By following these steps, you can effectively utilize the Kubernetes Downward API to make pod and container metadata available to your applications, enhancing their configurability and awareness of their execution context.

{% embed url="https://medium.com/@mkdev/what-is-kubernetes-downward-api-and-why-you-might-need-it-0b49a9ceaaca" %}

