# Image build: Kaniko
The main step of CI chain is a kaniko build. [Kaniko](https://github.com/GoogleContainerTools/kaniko) is a well-known tool for building docker images inside a container without docker itself.
Kaniko is called from the [Github Action workflow](GitHub_Action_Workflow.md)
using a cascade of Argo workflows and their templates (see the schem below).
## Kaniko cwft
### Invocation order
### cache
### repository proxy
