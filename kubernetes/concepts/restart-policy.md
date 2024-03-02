# Restart Policy

In Kubernetes, a pod's restart policy determines how the Kubernetes control plane behaves when a container within the pod terminates. Here's a breakdown of the available restart policies and their use cases:

**1. Always:**

* **Description:** This is the **default behavior** for pods. Kubernetes will **continuously restart** the container(s) within the pod whenever they exit, regardless of the exit code.
* **Use Case:** Use this for critical applications that must be running at all times, even if they encounter errors.
* **Example:** A web server pod that needs to be available for requests even if the web server process crashes due to unexpected errors.

**2. OnFailure:**

* **Description:** The container will be restarted **only if it exits with a non-zero exit code**, indicating an error. If the container exits gracefully with a zero exit code, it won't be restarted.
* **Use Case:** Use this for applications designed to perform a specific task and then exit. It ensures the container runs to completion if successful but prevents endless restarts if errors occur.
* **Example:** A script that processes a batch of data and then exits. If the script encounters errors (non-zero exit code), the pod will be restarted to attempt processing again.

**3. Never:**

* **Description:** Kubernetes will **not automatically restart** the container(s) within the pod after they terminate, regardless of the exit code.
* **Use Case:** Use this for one-time jobs or applications designed to run until completion without requiring restarts.
* **Example:** A pod that runs a migration script to convert data from one format to another. Once the script finishes successfully (exits with a zero code), there's no need to restart the pod.

**Choosing the Right Policy:**

The appropriate restart policy depends on your application's requirements:

* **Always:** Choose for critical services that must be continuously available.
* **OnFailure:** Choose for applications designed to complete tasks and potentially retry on errors.
* **Never:** Choose for one-time jobs or applications that manage their own lifecycle.

**Additional Considerations:**

* **Exit Codes:** When using "OnFailure," ensure your application uses appropriate exit codes to signal success (0) or failure (non-zero).
* **Backoff Strategy:** Kubernetes implements an exponential backoff strategy for restarts with the "Always" or "OnFailure" policy. This means the time between restart attempts increases after each failure to avoid overwhelming the system.

**Remember:** Selecting the correct restart policy helps ensure your pods behave as intended and achieve their desired functionality within your Kubernetes cluster.
