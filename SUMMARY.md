# Table of contents

* [Cloud Native Guide](README.md)

## Guides

* [Cloud Native](guides/cloud-native.md)
* [Software Engineering Concepts](guides/software-engineering-concepts/README.md)
  * [Multi Tenancy](guides/software-engineering-concepts/multi-tenancy.md)
  * [DNS](guides/software-engineering-concepts/dns.md)
  * [Protocols](guides/software-engineering-concepts/protocols.md)
  * [Cache](guides/software-engineering-concepts/cache.md)
  * [Reverse Proxy](guides/software-engineering-concepts/reverse-proxy.md)
  * [Basics](guides/software-engineering-concepts/basics.md)
* [Cheat Sheets](guides/cheat-sheets.md)
* [DevOps](guides/devops/README.md)
  * [Usual tasks](guides/devops/usual-tasks.md)
* [Observability](guides/observability/README.md)
  * [EBPF](guides/observability/ebpf.md)
  * [Tracing](guides/observability/tracing.md)
* [SRE](guides/sre.md)
* [DevOps Tools](guides/devops-tools.md)
* [CI CD](guides/ci-cd.md)
* [Kubernetes](guides/kubernetes/README.md)
  * [cluster setup](guides/kubernetes/cluster-setup.md)
  * [K3s](guides/kubernetes/k3s.md)
  * [Service Mesh](guides/kubernetes/service-mesh.md)
* [DevSecOps](guides/devsecops/README.md)
  * [Tools](guides/devsecops/tools.md)
  * [API security](guides/devsecops/api-security.md)
  * [Security Scanning](guides/devsecops/security-scanning.md)
  * [Preventing supply chain attacks](guides/devsecops/preventing-supply-chain-attacks.md)
  * [SBOM](guides/devsecops/sbom.md)
* [Git](guides/git/README.md)
  * [Rebase](guides/git/rebase.md)
* [Containers](guides/docker/README.md)
  * [Distroless](guides/docker/distroless.md)
  * [Scratch Image](guides/docker/scratch-image.md)
  * [Container Registry](guides/docker/container-registry.md)
* [Coding Practices](guides/coding-practices/README.md)
  * [MonoRepos](guides/coding-practices/monorepos.md)
* [Linux](guides/linux/README.md)
  * [Tools](guides/linux/tools/README.md)
    * [File Based](guides/linux/tools/file-based/README.md)
      * [Rsync](guides/linux/tools/file-based/rsync.md)
  * [Text Manipulation](guides/linux/text-manipulation.md)
  * [File system](guides/linux/file-system.md)
  * [Tmux](guides/linux/tmux.md)
  * [Boot process](guides/linux/boot-process.md)
  * [Editors](guides/linux/editors/README.md)
    * [Vim](guides/linux/editors/vim.md)
* [Hashicorp](guides/hashicorp/README.md)
  * [Terraform](guides/hashicorp/terraform.md)
  * [Boundary](guides/hashicorp/boundary.md)
  * [Vault](guides/hashicorp/vault.md)
* [Languages](guides/languages/README.md)
  * [Golang](guides/languages/golang/README.md)
    * [Concurrency](guides/languages/golang/concurrency.md)
    * [Basics](guides/languages/golang/basics.md)
  * [Python](guides/languages/python/README.md)
    * [Logging](guides/languages/python/logging.md)
  * [Bash](guides/languages/bash.md)
* [Gitops](guides/gitops.md)
* [Micro Services](guides/micro-services.md)
* [Platform Engineering](guides/platform-engineering/README.md)
  * [Internal Developer Platforms](guides/platform-engineering/internal-developer-platforms.md)
* [Performance Engineering](guides/performance-engineering.md)
* [AWS](guides/aws/README.md)
  * [Messaging/Streaming Services](guides/aws/messaging-streaming-services/README.md)
    * [Kinesis](guides/aws/messaging-streaming-services/kinesis.md)
  * [KMS](guides/aws/kms.md)
  * [IAM](guides/aws/iam.md)
  * [Step Workflows](guides/aws/step-workflows.md)
* [GraphQL](guides/graphql.md)
* [Grpc](guides/grpc.md)
* [Troubleshooting](guides/troubleshooting.md)
* [Databases](guides/databases/README.md)
  * [Stored Procedures](guides/databases/stored-procedures.md)
  * [Normalization](guides/databases/normalization.md)
* [Security](guides/security/README.md)
  * [Supply Chain Security](guides/security/supply-chain-security.md)
  * [Zero Trust](guides/security/zero-trust.md)
* [Non functional requirements](guides/non-functional-requirements/README.md)
  * [Scaling](guides/non-functional-requirements/scaling.md)
* [Networking](guides/networking/README.md)
  * [TLS](guides/networking/tls.md)
* [Event Driven Architecture](guides/event-driven-architecture.md)
* [ChatGpt](guides/chatgpt.md)
* [WASM](guides/wasm/README.md)
  * [Docker + WASM](guides/wasm/docker-+-wasm.md)
  * [Kubernetes + WASM](guides/wasm/kubernetes-+-wasm.md)

## System design concepts

* [Basics](system-design-concepts/basics.md)
* [Architecture](system-design-concepts/architecture.md)
* [Authentication](system-design-concepts/authentication/README.md)
  * [JWT](system-design-concepts/authentication/jwt.md)
  * [Oauth2](system-design-concepts/authentication/oauth2.md)
* [Performance](system-design-concepts/performance/README.md)
  * [Rate Limiting](system-design-concepts/performance/rate-limiting.md)
* [Idempotency](system-design-concepts/idempotency.md)
* [12 factor app](system-design-concepts/12-factor-app.md)
* [Solutions Architecture](system-design-concepts/solutions-architecture.md)

## 🎡 Kubernetes

* [Concepts](kubernetes/articles-videos/README.md)
  * [Best  Practices](kubernetes/concepts/best-practices/README.md)
    * [Security](kubernetes/concepts/best-practices/security.md)
    * [Deployment options](kubernetes/concepts/best-practices/deployment-options.md)
  * [Deployments](kubernetes/concepts/deployments.md)
  * [Security](kubernetes/concepts/security.md)
  * [Logging](kubernetes/concepts/logging.md)
  * [Garbage Collection](kubernetes/concepts/garbage-collection.md)
  * [Scaling](kubernetes/concepts/scaling.md)
  * [Setting up cluster](kubernetes/articles-videos/setting-up-cluster.md)
  * [Services](kubernetes/concepts/services/README.md)
    * [Endpoint Slices](kubernetes/concepts/services/endpoint-slices.md)
  * [Authentication/Authorization](kubernetes/articles-videos/authentication-authorization.md)
  * [Custom Controllers](kubernetes/concepts/custom-controllers.md)
  * [Operators](kubernetes/concepts/operators.md)
  * [Secrets](kubernetes/articles-videos/secrets.md)
* [Tools](kubernetes/tools/README.md)
  * [Auditing](kubernetes/tools/auditing/README.md)
    * [Checkov](kubernetes/tools/auditing/checkov.md)
  * [Chaos Engineering](kubernetes/tools/chaos-engineering.md)
  * [Templating/patching](kubernetes/tools/templating-patching/README.md)
    * [Kustomize](kubernetes/tools/templating-patching/kustomize.md)
    * [Helm](kubernetes/tools/templating-patching/helm.md)
  * [Multiple tools](kubernetes/tools/multiple-tools.md)
  * [Secrets Management](kubernetes/tools/secrets-management.md)
  * [Pipeline/Workflows](kubernetes/tools/pipeline-workflows/README.md)
    * [Argo Workflows](kubernetes/tools/pipeline-workflows/argo-workflows.md)
    * [Tekton Pipelines](kubernetes/tools/pipeline-workflows/tekton-pipelines.md)
    * [Jenkins](kubernetes/tools/pipeline-workflows/jenkins.md)
  * [Progressive Delivery](kubernetes/tools/progressive-delivery/README.md)
    * [Argo Rollouts](kubernetes/tools/progressive-delivery/argo-rollouts.md)
  * [Ingress](kubernetes/tools/ingress/README.md)
    * [Traefik](kubernetes/tools/ingress/traefik.md)
  * [Gitops](kubernetes/tools/gitops/README.md)
    * [Argo CD](kubernetes/tools/gitops/argo-cd.md)
  * [Service Mesh](kubernetes/tools/service-mesh/README.md)
    * [Linkerd](kubernetes/tools/service-mesh/linkerd.md)
  * [Container Builds](kubernetes/tools/container-builds/README.md)
    * [Kaniko](kubernetes/tools/container-builds/kaniko.md)
    * [Bazel](kubernetes/tools/container-builds/bazel.md)
  * [Logging](kubernetes/tools/logging/README.md)
    * [Fluentbit](kubernetes/tools/logging/fluentbit.md)
  * [Image Signing](kubernetes/tools/image-signing.md)
  * [Federation](kubernetes/tools/federation.md)
  * [Client-go](kubernetes/tools/client-go.md)
  * [Security Scanning](kubernetes/tools/security-scanning.md)
  * [Context Switching](kubernetes/tools/context-switching.md)
  * [Convertors](kubernetes/tools/convertors.md)
  * [Validation](kubernetes/tools/validation/README.md)
    * [Datree](kubernetes/tools/validation/datree.md)

## Tutorial Videos

* [Databases](tutorial-videos/databases/README.md)
  * [Postgresql](tutorial-videos/databases/postgresql.md)
* [Programming Languages](tutorial-videos/programming-languages/README.md)
  * [GoLang](tutorial-videos/programming-languages/golang.md)

## Articles

* [Blogs](articles/blogs.md)
* [Certifications](articles/certifications/README.md)
  * [AWS](articles/certifications/aws/README.md)
    * [Solutions architect associate](articles/certifications/aws/solutions-architect-associate.md)
* [Security](articles/untitled.md)
* [AWS](articles/aws/README.md)
  * [ECS](articles/aws/ecs.md)
  * [Lambda](articles/aws/lambda.md)
* [Docker](articles/docker/README.md)
  * [Docker Swarm Part 1: Creating swarm from container image](articles/docker/docker-swarm-part-1-creating-swarm-from-container-image.md)
* [Automation:](articles/automation/README.md)
  * [Postman](articles/automation/postman.md)
* [Serverless](articles/serverless.md)
* [Python](articles/python.md)
* [Monitoring](articles/monitoring.md)
* [Github](articles/github.md)
* [Ansible/Puppet](articles/ansible-puppet.md)
* [Kubernetes](articles/kubernetes/README.md)
  * [EKS](articles/kubernetes/eks.md)
* [Coding practices:](articles/coding-practices.md)
* [CI/CD](articles/ci-cd/README.md)
  * [Continuous Integration](articles/ci-cd/continuous-integration.md)
* [Web3](articles/web3.md)
* [Flutter](articles/flutter.md)

## Playlists

* [K8s](playlists/k8s.md)

***

* [Work tips](work-tips/README.md)
  * [Time management](work-tips/time-management.md)

## STACK OVERFLOW

* [Devops](stack-overflow/devops.md)
* [Linux](stack-overflow/linux.md)
* [Docker](stack-overflow/docker.md)
* [Python](stack-overflow/python.md)
* [AWS](stack-overflow/aws/README.md)
  * [SSM](stack-overflow/aws/ssm.md)

## REDDIT

* [Subreddits](reddit/subreddits.md)

## Github Pages

* [Interview Questions](github-pages/untitled.md)
* [Repos](github-pages/repos.md)
* [Guides](github-pages/guides.md)
* [awesome](github-pages/awesome.md)

## MY NOTES

* [AWS](my-notes/untitled/README.md)
  * [Billing and Cost Management](my-notes/untitled/billing-and-cost-management.md)
  * [Route53](my-notes/untitled/route53.md)
  * [Cloudtrail](my-notes/untitled/cloudtrail.md)
  * [EFS](my-notes/untitled/efs.md)
  * [EKS](my-notes/untitled/eks.md)
  * [ECS](my-notes/untitled/ecs/README.md)
    * [ECS Fargate blog](my-notes/untitled/ecs/ecs-fargate.md)
    * [Normal ECS](my-notes/untitled/ecs/normal-ecs.md)
  * [Cloudformation](my-notes/untitled/cloudformation.md)
  * [CDK](my-notes/untitled/cdk.md)
  * [Xray](my-notes/untitled/xray.md)
  * [S3](my-notes/untitled/untitled/README.md)
    * [Static Website](my-notes/untitled/untitled/static-website.md)
* [GCP](my-notes/gcp/README.md)
  * [Cloud Run](my-notes/gcp/cloud-run.md)
* [CI/CD](my-notes/ci-cd/README.md)
  * [Terraform](my-notes/ci-cd/terraform.md)
  * [Jenkins](my-notes/ci-cd/jenkins.md)
* [Linux](my-notes/linux/README.md)
  * [LPIC](my-notes/linux/lpic/README.md)
    * [LPIC-1](my-notes/linux/lpic/lpic-1.md)
  * [Networking](my-notes/linux/networking.md)
  * [Gotchas](my-notes/linux/gotchas.md)

## Packages/Binaries <a href="#packages" id="packages"></a>

* [Python](packages/packages.md)
* [Devops](packages/devops.md)

## 🎤 PODCASTS

* [Tech guidance](podcasts/tech-guidance.md)

## MASTERY PLAN

* [PLAN](mastery-plan/plan/README.md)
  * [b's](mastery-plan/plan/bs/README.md)
    * [AWS CDK - python](mastery-plan/plan/bs/aws-cdk-python.md)
    * [Elastic Stack installation on AWS Ubuntu 18.04 LTS](mastery-plan/plan/bs/elastic-stack-installation-on-aws-ubuntu-18.04-lts.md)
    * [Docker Swarm Part 1: HTTPS Nginx reverse proxy to the API](mastery-plan/plan/bs/docker-swarm-part-1-https-nginx-reverse-proxy-to-the-api.md)
    * [Merging multiple repo's into one](mastery-plan/plan/bs/merging-multiple-repos-into-one.md)
    * [Migrating to AWS](mastery-plan/plan/bs/migrating-to-aws.md)
    * [Packaging python code and uploading to PyPi](mastery-plan/plan/bs/packaging-python-code-and-uploading-to-pypi.md)
    * [Vscode keybindings](mastery-plan/plan/bs/vscode-keybindings.md)
    * [Starred Git repo](mastery-plan/plan/bs/starred-git-repo.md)
    * [Pytest](mastery-plan/plan/bs/pytest.md)

## NOTES

* [Links](notes/links.md)
* [Devops toolkit](notes/devops-toolkit.md)
* [Network Troubleshooting](notes/network-troubleshooting.md)
* [Nested Cloudformation Stack](notes/nested-cloudformation-stack/README.md)
  * [Links](notes/nested-cloudformation-stack/links.md)

## Learn by Images

* [Fargate](learn-by-images/fargate.md)
* [REST API](learn-by-images/rest-api.md)
* [Observability](learn-by-images/observability.md)
* [AWS CODE BUILD](learn-by-images/aws-code-build.md)
* [Docker](learn-by-images/docker/README.md)
  * [Container Runtimes](learn-by-images/docker/container-runtimes.md)
  * [Docker Internals](learn-by-images/docker/docker-internals.md)
  * [Dockerfile](learn-by-images/docker/dockerfile.md)
* [AWS CDK](learn-by-images/aws-cdk.md)
* [Networking:](learn-by-images/networking.md)
* [Security](learn-by-images/security.md)
* [Kotlin](learn-by-images/kotlin.md)

## Command line

* [Shortcuts](command-line/shortcuts.md)

## Video Session/notes

* [Well Architectured Framework](video-session-notes/well-architectured-framework.md)

## Certifications

* [Kubernetes](certifications/kubernetes/README.md)
  * [CKAD](certifications/kubernetes/ckad/README.md)
    * [Observability](certifications/kubernetes/ckad/observability.md)
    * [Services and networking](certifications/kubernetes/ckad/services-and-networking.md)

## Best Practices

* [Continuous Integration](best-practices/continuous-integration.md)

## Motivation

* [Motivation](motivation/motivation.md)
