
The CG DevX reference implementation provides image and helm chart repository capabilities using Harbor.

To access Harbor, follow the link in the platform GitOps repository readme file (`README.md`),
or provided by operators (AKA a platform team).
Harbor is configured to use Vault as its OIDC provider and will automatically redirect you to the Vault login page,
which will look like this:

<!-- Crop images -->
![vault_login_userpass.png](../../assets/vault_login_userpass.png)

Each workload in CG DevX has a project in Harbor associated with it.
A project contains all the repositories for a workload.
Control (RBAC) is applied to projects, so that only users who belong to a specific team can access it.

![harbor_workload.png](../../assets/harbor_workload_project.png)

Delivery pipelines provided by CG DevX are pre-configured to work with the registry.

Also, the registry provides static analysis of vulnerabilities in images through the integration with Aqua Security Trivy.

![harbor_workload_repo.png](../../assets/harbor_workload_repo.png)
