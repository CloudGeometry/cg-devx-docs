## Main Benefits to Operators

- **Better visibility and faster validation of changes**:
    - Identify and fix security issues before they reach production.
    - Health checks and rollback systems allow quick resolution of deployment failures.
- **Higher quality by**:
    - Standardizing cloud building blocks.
    - Automating configuration and infrastructure to lower the potential for environment-specific problems.
    - Logging, monitoring, and alerting available out of the box increase the operability of services.
    - Full automation of the provisioning process enables business continuity.
- **Built-in guardrails and compliance checks**:
    - Policies defined in code and validated against infrastructure and applications both prior to deployment and
      continuously at runtime.
    - Enables developer teams to be accountable for the security of their services.

## GitOps

CG DevX uses a GitOps approach for platform management. The Platform GitOps repository created by the setup process has
two main parts:

- `/gitops_pipelines`: Clusters and core services GitOps configurations.
- `/terraform`: Infrastructure as Code & Configuration as Code for all the cloud services, Git provider, secrets, user
  management, and core platform services.

We find such a combination of IaC and K8s manifests in the same repository feasible. For those who may have strict
requirements on access control, it could be divided into two separate repositories.

## Runtime

CG DevX services can be virtually divided into two groups: core services responsible for platform functionality and
orchestration (control plane), and runtime where users' workloads will be executed. Runtime contains only the services
responsible for workload delivery and configuration management.

CG DevX supports different runtime deployment options based on workload needs. The most common options are:

- **Using one cluster**: Same cluster for control plane and runtime. This cluster should be considered the Control
  Center Cluster (CC Cluster). Default option provided with reference implementation.
- **Multiple K8S clusters**: CC Cluster (management cluster) plus runtime cluster(s) within one region.
- **Multi-region installation**: Usually with one control plane cluster per region.
- **Multi-cloud installation**: Can combine all the options above.

![runtime_multicluster](../assets/diagrams.drawio)

Additional K8s cluster runtimes are usually used when a workload requires "hard isolation," e.g., dedicated production
environment, dedicated tenant installation, etc.

In this deployment option, runtime has its own services responsible for workload delivery and configuration management.
Depending on workload isolation requirements, other services like log management could be instantiated.

![runtime_multicluster_workflow](../assets/diagrams.drawio)

## Workload Isolation

CG DevX uses a combination of Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC) to ensure
proper access control within the cluster and outside the cluster when connecting to non-cluster resources. Core
components are provisioned within a secure boundary with management access limited to the operators (platform team),
while using fine-grained access to the underlying infrastructure.

> We strongly recommend operators apply changes to the platform or underlying cloud infrastructure using the GitOps
> approach, and not to use users with "write" access to the infrastructure. While we provide drift detection mechanisms,
> the potential impact of manual infrastructure and configuration changes could not be fully mitigated.

The default workload management strategy is "soft multi-tenancy," where a cluster is shared by multiple teams and
workloads, meaning that you trust your teams and their workloads. Each workload is provisioned within a virtual
environment forming a secure perimeter isolated from other workloads. Each workload has its own set of secrets
accessible only by the workload and teams responsible for it. Developer teams do not have direct modify access to
underlying infrastructure or live environment configuration settings and should apply changes using workload GitOps.

![runtime](../assets/diagrams.drawio)

## BC/DR

CG DevX is designed following High Availability and Cloud Elasticity principles and supports single region multiple
availability zones. The default Disaster Recovery (DR) strategy is Backup & Restore, with all artifacts offloaded out of
the cluster to 1+ regions.

RTO and RPO depend on stateful resources used by the workload. The CG DevX Platform can be provisioned in under 1 hour,
while the app runtime module itself most typically could accept and serve workloads in ~15 minutes. Cold (pilot light)
and Warm standby can be achieved by provisioning the CG DevX module set in another region and changing default
autoscaling policies. Most CG DevX components support active-active scenarios. The feasibility of those scenarios
depends on workload requirements for stateful resources, which are considered to be non-platform resources, as CG DevX
is designed to primarily serve stateless workloads.
