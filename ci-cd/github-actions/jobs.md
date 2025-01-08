# Jobs

A **job** is a set of **steps** in a workflow that is executed on the same **runner**. Each step is either a shell script that will be executed, or an **action** that will be run

* **by default**, jobs have no dependencies and **run in parallel**.
  * You can configure a job's dependencies with other jobs;&#x20;







Steps are **executed in order and are dependent on each other**.&#x20;

Since each step is executed on the same runne&#x72;**, you can share data from one step to another**

