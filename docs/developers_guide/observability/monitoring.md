
CG DevX reference implementation provides monitoring and log management capabilities using Grafana, Prometheus, and Loki.

Grafana serves as a visualization tool for both metrics and logs.
To access Grafana, follow the link in platform GitOps repository readme file (`README.md`),
or provided by operators (AKA a platform team).

Grafana is configured to use Vault as OIDC provider.

![grafana_login.png](../../assets/grafana_login.png)

You need to press `Sign in with Vault` button which will redirect you to Vault login page,
which will look like this

![vault_login.png](../../assets/vault_login_userpass.png)

Control (RBAC) is applied to limit access to workload dashboards.

CG DevX has pre-installed K8s specific [dashboards](dashboards.md),
plus language- / framework-specific [dashboards](dashboards.md) that could be used as templates when creating workload-specific dashboard.
