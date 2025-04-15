# Start Here

Kubernetes is a <mark style="background-color:purple;">portable, extensible, open source platform for managing containerized workloads</mark> and services, that facilitates both declarative configuration and automation

The name Kubernetes originates from Greek, meaning helmsman or pilot. K8s as an abbreviation results from counting the eight letters between the "K" and the "s".&#x20;

<mark style="background-color:purple;">**Google open-sourced the Kubernetes project in 2014**</mark>

K8s is the pacific ocean of concepts. The more deeper you go the more complex stuff you will land on. Below is the overview of how you can approach the concepts one by one. IMO atleast the core components, infra control plane,rbac and networking should be crystal clear in your mind. You can leverage those mappings to build with the next components like scaling, monitoring, security, dr etc

<figure><img src="../../.gitbook/assets/image (270).png" alt=""><figcaption></figcaption></figure>

## **Kubernetes provides you with:**

* **Service discovery and load balancing** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
* **Storage orchestration** Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
* **Automated rollouts and rollbacks** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
* **Automatic bin packing** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
* **Self-healing** Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
* **Secret and configuration management** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.
* **Batch execution** In addition to services, Kubernetes can manage your batch and CI workloads, replacing containers that fail, if desired.
* **Horizontal scaling** Scale your application up and down with a simple command, with a UI, or automatically based on CPU usage.
* **IPv4/IPv6 dual-stack** Allocation of IPv4 and IPv6 addresses to Pods and Services
* **Designed for extensibility** Add features to your Kubernetes cluster without changing upstream source code.

## What Kubernetes is not <a href="#what-kubernetes-is-not" id="what-kubernetes-is-not"></a>



Kubernetes is <mark style="background-color:red;">not a traditional, all-inclusive PaaS (Platform as a Service) system.</mark> Since Kubernetes operates at the container level rather than at the hardware level, it provides some generally applicable features common to PaaS offerings, such as deployment, scaling, load balancing, and lets users integrate their logging, monitoring, and alerting solutions. However, Kubernetes is not monolithic, and these default solutions are optional and pluggable. Kubernetes provides the building blocks for building developer platforms, but preserves user choice and flexibility where it is important.

Kubernetes:

* Does not limit the types of applications supported. Kubernetes aims to support an extremely diverse variety of workloads, including stateless, stateful, and data-processing workloads. If an application can run in a container, it should run great on Kubernetes.
* <mark style="background-color:red;">Does not deploy source code and does not build your application.</mark> Continuous Integration, Delivery, and Deployment (CI/CD) workflows are determined by organization cultures and preferences as well as technical requirements.
* <mark style="background-color:red;">Does not provide application-level services, such as middleware (for example, message buses), data-processing frameworks (for example, Spark), databases (for example, MySQL), caches, nor cluster storage systems (for example, Ceph) as built-in services</mark>. Such components can run on Kubernetes, and/or can be accessed by applications running on Kubernetes through portable mechanisms, such as the [Open Service Broker](https://openservicebrokerapi.org/).
* <mark style="background-color:red;">Does not dictate logging, monitoring, or alerting solutions</mark>. It provides some integrations as proof of concept, and mechanisms to collect and export metrics.
* <mark style="background-color:red;">Does not provide nor mandate a configuration language/system (for example, Jsonnet)</mark>. It provides a declarative API that may be targeted by arbitrary forms of declarative specifications.
* Does not provide nor adopt any comprehensive machine configuration, maintenance, management, or self-healing systems.
* Additionally, <mark style="color:red;">Kubernetes is not a mere orchestration system</mark>. In fact, it eliminates the need for orchestration. The technical definition of orchestration is execution of a defined workflow: first do A, then B, then C. In contrast<mark style="background-color:red;">, Kubernetes comprises a set of independent, composable control processes that continuously drive the current state towards the provided desired state.</mark> It shouldn't matter how you get from A to C. Centralized control is also not required. This results in a system that is easier to use and more powerful, robust, resilient, and extensible.
