# Image build: Kaniko
The main step of CI chain is a kaniko build. [Kaniko](https://github.com/GoogleContainerTools/kaniko) is a well-known tool for building docker images inside a container without docker itself.
Kaniko is called from the [Github Action workflow](github_action_workflow.md)
using a [cascade](build_routine.md) of Argo workflows and their templates.
## Kaniko cwft
[kaniko clusterworkflow template](<!--link to file in github-->) performs the checkout of workload's repo as a hardwired artifact, builds a tar-image of service and put it as an output artifact to the pre-configured artifact registry (S3-bucket in AWS usage case). 
  
  While it's possible to utilize Kaniko itself to check out the repository and place the final artifact in the Harbor registry, this approach was intentionally avoided for the following reasons:

- reduce dependence from kaniko;
- logicaly separate build and push processes.

By default cache for RUN layers and mirrors of popular repositories for base images are used, but this behavior could be switched using corresponding env variables in [Github Action workflow](github_action_workflow.md). Cache stores on per-service basis in Harbor, as well as proxy-repositories are also special projects in Harbor. To bypass the long-term kaniko bugwith registry URLs dockerfiles are edited right in place during workflow execution to replace the basic images names with a combintaion of Harbor proxy project name and the name of the image. 
<!-- snippet from GH with kaniko executor args-->
