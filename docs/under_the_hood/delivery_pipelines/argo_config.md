# Argo workflows configuration
Argo workflows configuration is divided by 2 parts: one is a helm chart _values_ in
[application.yaml](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/argo-workflows/application.yaml) and
basic entities declaration in cluster repo  common for all the cluster installation, another one is a
workload-specific kubernetes entities declaration in the `wl-<workload name>-dev`
namespace, executing through kustomize scenario. 

Helm chart _values_ defined in
[application.yaml](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/argo-workflows/application.yaml)
consists of several sections, one for each component of Argo Workflows: 
- _controller_ for Workflow Controller
- _executor_ for Executor
- _server_ for Argo Workflow Server
- _artifactRepository_ for default artifact repository

SSO declaration is a part of _server_ section; also every section has it's own
_rbac_ parameters. 

All the Argo Workflow components images are pulling through Harbor registry
proxy. To dive deep into the helm chart values see
[https://github.com/argoproj/argo-helm/blob/main/charts/argo-workflows/values.yaml](https://github.com/argoproj/argo-helm/blob/main/charts/argo-workflows/values.yaml).

### Namespaces
Namespaces names based on the name of workloads are not known before the workload created, so only `argo` namesapace noticed for provisioning by helm; all the others are provisioned by kustomize scenario in _-gitops_ repo of workload.
