
CG DevX reference implementation provides continuous integration capabilities
using Git native pipelines (GitHub Actions, GitLab Pipelines), and ArgoWorkflow.

ArgoWorkflow is used to integrate natively with K8s services.

To access ArgoWorkflow, follow the link in platform GitOps repository readme file (`README.md`),
or provided by operators (AKA a platform team).

ArgoWorkflow is configured to use Vault as OIDC provider.

![argoworflow_login.png](../../assets/argoworflow_login.png)

You need to press `Login` button on the left side which will redirect you to Vault login page,
which will look like this

![vault_login.png](../../assets/vault_login_userpass.png)

Each workload in CG DevX has a namespace in ArgoWorkflow associated with it.
Control (RBAC) is applied to projects, so that only user of a specific team could access it.

![argoworflow_login.png](../../assets/argoworflow_workload.png)

CG DevX ArgoWorkflow pipelines are pre-configured to work with static code analysis tools,
linters, security scanners, and image registries.
It also uses automated cleanup to remove old artifacts.
