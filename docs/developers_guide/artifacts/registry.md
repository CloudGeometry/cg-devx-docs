
CG DevX reference implementation provides image and helm chart repository capabilities using Harbor.

Each workload in CG DevX has a project in Harbor associated with it.
A project contains all repositories of a workload.
Control (RBAC) is applied to projects, so that only user of a specific team could access it.

![harbor_workload.png](../../assets/harbor_workload_project.png)

Delivery pipelines provided by CG DevX are pre-configured to work with registry.

Also, registry provides static analysis of vulnerabilities in images through the integration with Aqua Security Trivy.

![harbor_workload_repo.png](../../assets/harbor_workload_repo.png)