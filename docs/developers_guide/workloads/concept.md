Workload could be described as one or more applications, systems, or solutions fulfilling specific business goals. In
some cases those terms could be used interchangeably.
Application / system / solution is composed of services, where service is the smallest independently deployable and
managed unit performing an isolated set of business functions.

To be successfully integrated with CG DevX workload, and all of its services should:

- Be single process, or managed by single process
- Be stateless (we do not recommend putting workloads working with or storing business-critical data that require strong consistency), meaning
    - No in memory state that could not be recovered/restored/recalculated
    - No local file system/storage that could not be recovered/restored/recreated
- Support graceful shutdown
- Be packed in a container according to guidelines provided by our architects
    - Base image
    - Log streams
    - Metrics exposure
    - Liveness/readiness probes
    - Config is stored in the environment
