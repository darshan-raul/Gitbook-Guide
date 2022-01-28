# Github Actions



A GitHub Event triggers a **Workflow**. A Workflow is run on a **Runner**.

* A **Workflow** has one or more jobs (that by default run in parallel)
* A **Job** has one or more steps that execute on the same runner. Steps in the same job can pass data between them
* A **Step** (individual task) has 1 action
* An **Action** is the smallest standalone components combined into steps to create a job
* A **Runner** is a server that has the GitHub Actions runner application installed which listens for jobs, runs one job at a time, reports progress, logs results



Workflow triggers are events that cause a workflow to run. These events can be:

* Events that occur in your workflow's repository
* Events that occur outside of GitHub and trigger a `repository_dispatch` event on GitHub
* Scheduled times
* Manual

For example, you can configure your workflow to run when a push is made to the default branch of your repository, when a release is created, or when an issue is opened.

**The following steps occur to trigger a workflow run:**

1. An event occurs on your repository. The event has an associated commit SHA and Git ref.
2. GitHub searches the `.github/workflows` directory in your repository for workflow files that are present in the associated commit SHA or Git ref of the event.
3.  A workflow run is triggered for any workflows that have `on:` values that match the triggering event. Some events also require the workflow file to be present on the default branch of the repository in order to run.

    **Each workflow run will use the version of the workflow that is present in the associated commit SHA or Git ref of the event.**&#x20;

## References:

{% embed url="https://deborah-digges.github.io/2020/10/14/Building-a-Github-action" %}

{% embed url="https://lo-victoria.com/series/github-actions" %}
