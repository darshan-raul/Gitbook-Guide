# Well Architectured Framework

It is a framework which helps cloud architects build secure, high-performing, resilient, and efficient infrastructure for their applications and workloads

5 pillars:

* **Security Pillar**
* **Operational Excellence Pillar**
* **Reliability Pillar**
* **Performance Efficiency Pillar**
* **Cost Optimization Pillar**

## Terms:

* A **component** is the **`code, configuration, and AWS Resources`** that together deliver against a requirement.&#x20;
* The term **workload** is used to identify a **`set of components that together deliver business value`**.&#x20;
* We think about **architecture** as being how components work together in a workload. How components communicate and interact is often the focus of architecture diagrams.
* **Milestones** mark key changes in your architecture as it evolves throughout the product life cycle (design, implementation, testing, go live, and in production).
* Within an organisation the **technology portfolio** is the collection of workloads that are required for the business to operate.

> When architecture workloads, you make trade-offs between pillars based on your business context.

## General Design principles:

* **Stop guessing your capacity needs**
* **Test systems at production scale**
* **Automate to make architectural experimentation easier**:
* **Allow for evolutionary architectures**: In a traditional environment, architectural decisions are often implemented as static, onetime events, with a few major versions of a system during its lifetime. As a business and its context continue to evolve, these initial decisions might hinder the system's ability to deliver changing business requirements. In the cloud, the capability to automate and test on demand lowers the risk of impact from design changes. This allows systems to evolve over time so that businesses can take advantage of innovations as a standard practice.
* **Drive architectures using data**: In the cloud, you can collect data on how your architectural choices affect the behavior of your workload. This lets you make factbased decisions on how to improve your workload.&#x20;
* **Improve through game days**: Test how your architecture and processes perform by regularly scheduling game days to simulate events in production. This will help you understand where improvements can be made and can help develop organizational experience in dealing with events.





### Interesting points:

* Traditionally teams use [TOGAF](http://pubs.opengroup.org/architecture/togaf9-doc/arch/?ref=wellarchitected-wp) or the [Zachman Framework](https://www.zachman.com/about-the-zachman-framework?ref=wellarchitected-wp) as part of an enterprise architecture capability.&#x20;
