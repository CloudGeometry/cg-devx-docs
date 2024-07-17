# CG DevX platform GitOps repository

When setting up CG DevX, the CLI tool creates your platform repository, which serves as the basis for GitOps operations.
CG DevX relies on the GitOps approach for platform management.

The `GitOps` repository has two main sections

- `/gitops_pipelines`: delivery pipeline configurations
- `/terraform`: infrastructure as code and configuration as code for all the cloud services, git provider, secrets and
  user management

The repository root `readme.md` file contains links to all core services:

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

The GitOps pipeline contains cluster configurations for multi cluster support (to be added later),
and ArgoCD manifests for all the core services.
The core services configuration is located at `/gitops-pipelines/delivery/clusters/cc-cluster/core-services/`.
An ArgoCD App of Apps pattern is used here.

Workloads are managed via an ApplicationSet that monitors
the `/gitops-pipelines/delivery/clusters/cc-cluster/workloads/` directory.
The default workload template is located
at `/gitops-pipelines/delivery/clusters/cc-cluster/workloads/workload-template.yaml`, and is used
by the [workload bootstrap command](../workload_management/cli_commands.md#bootstrap)

## IaC

Terraform is used as an implementation of IaC for CG DevX platform.
The Terraform code is located at `./terraform/`.
The configuration consists of five logical blocks providing resource and configuration management for:

1. git provider configuration and repositories
2. cloud resources
3. secrets
4. user management
5. core services

Logical blocks rely on high-order terraform modules defined under `/terraform/modules/`.
Those modules abstract away specifics of block implementation by providing a simple-to-use interface.
When the default behavior of a module cannot be changed through configuration, you can edit
modules achieve the expected behavior.

Terraform state files will be stored in a cloud provider specific storage backend.
The CLI tool creates storage using the cloud API, and is hardened,
where implementation is cloud provider specific, but will always try to:

- enable versioning
- enable delete protection
- limit access to the user whose credentials are used to install CG DevX, plus the role assumed by the PR Automation
  service (Atlantis).

### IaC PR Automation

All of our terraform management is automated with a tool called Atlantis, which integrates with your git pull requests.

When you make a change to a `*.tf` file, even a whitespace change, Atlantis will pick up that change.
Atlantis automatically runs `terraform plan` and `comment` back to PR with the details.
The pull request goes through the review proces, and when approved, changes can be applied by
commenting `atlantis apply`.
Atlantis will run `terraform apply`, merge the PR, and delete the branch.

Atlantis:

- enables code review for infrastructure changes
- allows doing infrastructure changes without credentials
- serves as audit log

Atlantis' configuration is described in `atlantis.yaml` file setting the correct execution order of logical blocks.

CG DevX default settings require at least one person besides the author of the PR to approve it.
This is not a default Atlantis configuration and is done as a part of security hardening.

You could change Atlantis
settings `/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/atlantis/application.yaml`

For more details, please see [Atlantis official documentation](https://www.runatlantis.io/docs/)
