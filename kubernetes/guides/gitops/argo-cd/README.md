# Argo CD

### Architecture:



<figure><img src="../../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

> Live example: [https://cd.apps.argoproj.io/applications](https://cd.apps.argoproj.io/applications?showFavorites=false\&proj=\&sync=\&health=\&namespace=\&cluster=\&labels=)

Below are some of the concepts that are specific to Argo CD.

* **Application** A group of Kubernetes resources as defined by a manifest. This is a Custom Resource Definition (CRD).
* **Application source type** Which **Tool** is used to build the application.
* **Target state** The desired state of an application, as represented by files in a Git repository.
* **Live state** The live state of that application. What pods etc are deployed.
* **Sync status** Whether or not the live state matches the target state. Is the deployed application the same as Git says it should be?
* **Sync** The process of making an application move to its target state. E.g. by applying changes to a Kubernetes cluster.
* **Sync operation status** Whether or not a sync succeeded.
* **Refresh** Compare the latest code in Git with the live state. Figure out what is different.
* **Health** The health of the application, is it running correctly? Can it serve requests?
* **Tool** A tool to create manifests from a directory of files. E.g. Kustomize. See **Application Source Type**.
* **Configuration management tool** See **Tool**.
* **Configuration management plugin** A custom tool.



### Gotchas:

* Argo CD project follows the same support scheme as Kubernetes but for N, N-1 while Kubernetes supports N, N-1, N-2 versions.
* Argo CD applications, projects and settings can be defined declaratively using Kubernetes manifests. These can be updated using `kubectl apply`, without needing to touch the `argocd` command-line tool.
  * Be sure to annotate your ConfigMap resources using the label `app.kubernetes.io/part-of: argocd`, otherwise Argo CD will not be able to use them.
* By default, deleting an application will not perform a cascade delete, which would delete its resources. You must add the finalizer if you want this behaviour - which you may well not want.
* You **can create an app that creates other apps**, which in turn can create other apps. This allows you to declaratively manage a group of apps that can be deployed and configured in concert.
* **Resources can be excluded from discovery and sync so that Argo CD is unaware of them**. For example, `events.k8s.io` and `metrics.k8s.io` are always excluded. Use cases:
  * You have temporal issues and you want to exclude problematic resources.
  * There are many of a kind of resources that impacts Argo CD's performance.
  * Restrict Argo CD's access to certain kinds of resources, e.g. secrets.



{% embed url="https://www.youtube.com/watch?v=fQ9846hRiFo" %}
