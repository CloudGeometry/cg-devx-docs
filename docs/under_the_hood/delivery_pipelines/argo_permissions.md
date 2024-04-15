# Argo workflows permission scopes
As noticed in [Argo workflows configuration](argo_config.md) chapter, 
two Argo workflows permission scopes: `argo` namespace  and `wl-<workload name>-dev`
namespace. Also there are some clusterroles defined by helm chart.  
`argo` namespace system accounts and roles are used to execute Argo Workflows
Server and Workflow controller.

## System accounts and roles
Each workload provides following system accounts and roles in `wl-<workload name>-dev`:

- `argo-admin` — RBAC map target to view, change and delete workflows 
- `argo-developer` — RBAC map target to view workflows
- `argo-workflow` — used to execute all the [CI chain](../../developers_guide/ci/build_routine.md) workflows 

<!-- link to wl-template-gitops here -->


`argo` namespace are provided with several special systemaccounts:

- `argo-server` for Argo Workflows Server
- `argo-workflow-controller` for Argo Workflow controller
- `argo-default-sa` — default account with the lowest permissions to login into Argo
  Workflows UI and is used before the RBAC rules are applied.

  To dive deep inside Argo Workflows RBAC, see:

  - [https://argo-workflows.readthedocs.io/en/stable/workflow-rbac/](https://argo-workflows.readthedocs.io/en/stable/workflow-rbac/)
  - [https://argo-workflows.readthedocs.io/en/stable/argo-server-sso/#sso-rbac](https://argo-workflows.readthedocs.io/en/stable/argo-server-sso/#sso-rbac)
