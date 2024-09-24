# Workflows

A **workflow** is a configurable automated process that will run one or more jobs.&#x20;

Workflows are defined by a YAML file checked in to your repository and&#x20;

* will run when triggered by an event in your repository
* &#x20;or they can be triggered manually,&#x20;
* or at a defined schedule.

Workflows are defined in the `.github/workflows` directory in a repository.

&#x20;<mark style="background-color:orange;">A repository can have multiple workflows, each which can perform a different set of tasks</mark>

### Gotchas

* You can reference a workflow within another workflow
* You can give the workflow file any name you like, but you must use `.yml` or `.yaml` as the file name extension.&#x20;
