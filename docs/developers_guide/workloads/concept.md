# Workload concept

A Workload can be described as one or more applications, systems, or solutions fulfilling specific business goals. In
some cases, those terms can be used interchangeably.
An application / system / solution is composed of services, where a service is the smallest independently deployable and
managed unit performing an isolated set of business functions.

## Prerequisites

To be successfully integrated with CG DevX workload, all of an application's services should:

- Be single process, or managed by a single process
- Be stateless (we do not recommend putting workloads working with or storing business-critical data that require strong
  consistency), meaning
    - No in memory state that cannot be recovered/restored/recalculated
    - No local file system/storage that cannot be recovered/restored/recreated
- Support graceful shutdown
- Be packed in a container according to guidelines
  regarding: <!-- "provided by our architects" should eventually be replaced by the content added to these docs. -->
    - Base image
    - Log streams
    - Metrics exposure
    - Liveness/readiness probes
    - Config is stored in the environment

## What's included

Workloads created by CG DevX come with complete CI/CD processes including automated builds,
container build & publishing, linting templates,
integration with different test frameworks, [GitOps definition and deployments](gitops_environments.md),
and version and release management.
CG DevX saves time, doing all the heavy-lifting by handling all the integrations,
and all you need to do is to fine-tune templates to your specific needs.  
