# Image build: Kaniko

The main step of CI chain is a Kaniko build. [Kaniko](https://github.com/GoogleContainerTools/Kaniko) is a well-known
tool for building docker images inside a container without docker itself.
Kaniko is called from the [GitHub Action workflow](github_action_workflow.md)
using a [cascade](build_routine.md) of Argo workflows and their templates.

## Kaniko cwft

[kaniko clusterworkflow template](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/argo-workflows/cluster-workflow-templates/Kaniko-s3-cwft.yaml)
performs the checkout of workload's repo as a hardwired artifact, builds a tar-image of service and put it as an output
artifact to the pre-configured artifact registry (S3-bucket in AWS usage case).

While it's possible to utilize Kaniko itself to check out the repository and place the final artifact in the Harbor
registry, this approach was intentionally avoided for the following reasons:

- reduce dependence from Kaniko;
- logically separate build and push processes.

By default, cache for RUN layers and mirrors of popular repositories for base images are used, but this behavior could be
switched using corresponding env variables in [GitHub Action workflow](github_action_workflow.md). Cache stores on
per-service basis in Harbor, as well as proxy-repositories are also special projects in Harbor. To bypass the long-term
Kaniko bug with registry URLs dockerfiles are edited right in place during workflow execution to replace the basic images
names with a combination of Harbor proxy project name and the name of the image.

A snippet
from  [kaniko clusterworkflow template](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/argo-workflows/cluster-workflow-templates/Kaniko-s3-cwft.yaml)
(Note the values transferred through workflow parameters):

```yaml
args:
  - --dockerfile={{workflow.parameters.dockerfile}}
  - --context=dir:///workspace/{{workflow.parameters.build-context}}
  - --no-push
  - --tar-path=/tmp/{{workflow.parameters.wl-service-name}}.tar
  - --registry-mirror={{workflow.parameters.kaniko-registry-mirror}}
  - --snapshot-mode=time
  - --use-new-run
  - --compressed-caching=false
  - --cache={{workflow.parameters.kaniko-cache}}
  - --cache-run-layers
  - --cache-repo={{workflow.parameters.kaniko-cache-repo}}/kaniko-cache/{{workflow.parameters.workload-name}}-{{workflow.parameters.wl-service-name}}
```
