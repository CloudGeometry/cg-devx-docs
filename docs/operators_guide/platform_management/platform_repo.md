# CG DevX platform GitOps repository

When running CLI tool to set up CG DevX it will create your platform repository.
CG DevX relies on GitOps approach for platform management.

The `GitOps` repository has two main sections

- `/gitops_pipelines`: delivery pipeline configurations
- `/terraform`: infrastructure as code & configuration as code for all the cloud services, git provider, secrets and
  user management

Repository root `readme.md` file will contain links to all core services:

| Application    | Namespace  | Description                        | URL (where applicable)                         |
|----------------|------------|------------------------------------|------------------------------------------------|
| Vault          | vault      | Secrets Management                 | https://vault.<cluster-name>.<domain-name>     |
| Argo CD        | argocd     | GitOps Continuous Delivery         | https://argocd.<cluster-name>.<domain-name>    |
| Argo Workflows | argo       | Application Continuous Integration | https://argo.<cluster-name>.<domain-name>      |
| Atlantis       | atlantis   | Terraform Workflow Automation      | https://atlantis.<cluster-name>.<domain-name>  |
| Harbor         | harbor     | Image & Helm Chart Registry        | https://harbor.<cluster-name>.<domain-name>    |
| Grafana        | monitoring | Observability                      | https://grafana.<cluster-name>.<domain-name>   |
| SonarQube      | sonarqube  | Code Quality                       | https://sonarqube.<cluster-name>.<domain-name> |
| Backstage      | backstage  | Portal                             | https://backstage.<cluster-name>.<domain-name> |

[Cert-manager](https://cert-manager.io/) with a [Let's Encrypt](https://letsencrypt.org/) ClusterIssuer for TLS
encryption is used to secure all public-facing services on Ingress. Certificates are requested automatically and will be
auto-renewed.

## GitOps pipelines

Contains cluster configuration for multi cluster support (to be added later)
and ArgoCD manifests for all the core services.
Core services configuration is located at `/gitops-pipelines/delivery/clusters/cc-cluster/core-services/`.
An ArgoCD App of Apps pattern is used here.

Workloads are managed via AppSet, that monitors `/gitops-pipelines/delivery/clusters/cc-cluster/workloads/` directory.
Default workload template is located
at `/gitops-pipelines/delivery/clusters/cc-cluster/workloads/workload-template.yaml` This template is used
by [workload bootstrap command](../workload_management/cli_commands.md#bootstrap)

## IaC

Terraform is used as an implementation of IaC for CG DevX platform.
Terraform code is located at `./terraform/`.
Configuration consists of five logical blocks providing resource and configuration management for:

1. git provider configuration and repositories
2. cloud resources
3. secrets
4. user management
5. core services

Logical blocks rely on high-order terraform modules defined under `/terraform/modules/`.
Those modules abstract away specifics of block implementation by providing a simple-to-use interface.
When the default behavior of module could not be changed through configuration,
modules could be edited to achieve expected behavior.

Terraform state files will be stored in a cloud provider specific storage backend.
Storage is created by CLI tool using cloud API, and is hardened,
where implementation is cloud provider specific, but will always try to:

- enable versioning
- enable delete protection
- limit access to the user which credentials are used to install CG DevX, plus role assumed by PR Automation service (
  Atlantis).

### IaC PR Automation

All of our terraform management is automated with a tool called Atlantis that integrates with your git pull requests.

When a change to a `*.tf` file, even a whitespace change, is introduced, it will be picked by Atlantis.
Atlantis will automatically run terraform plan and comment back to PR with the details.
PR goes through the review proces, and when approved, changes could be applied by commenting `atlantis apply`.
Atlantis will run `terraform apply`, merge PR, and delete branch.

Atlantis:

- enables code review for infrastructure changes
- allows doing infrastructure changes without credentials
- serves as audit log

Atlantis' configuration is described in `atlantis.yaml` file setting the correct execution order of logical blocks.

CG DevX default settings require at least one person except the author of PR approve it.
This is not a default Atlantis configuration and is done as a part of security hardening.

You could change Atlantis
settings `/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/atlantis/application.yaml`

For more details, please see [Atlantis official documentation](https://www.runatlantis.io/docs/)