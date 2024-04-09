# Cost Management

Our goal is to improve cost visibility and provide cost savings recommendations for the platform, and it's core
services. To do so CG DevX reference architecture incorporates cloud and in cluster resource labeling / tagging, and
integrates with real-time cost visibility and insights tools. By default, free version
of [KubeCost](https://www.kubecost.com/) is installed with the platform and could be replaced
with [OpenCost](https://www.opencost.io/) or other tools from our integration catalog.

To properly manage costs user of CG DevX should have full scale cost management (visibility, analysis, optimization,
governance) AKA FinOps solution.

> CG DevX reference architecture could not and should not replace your primary cost management solution.

## Cost visibility & attribution

As with many practices in operations, implementing a labeling / tagging strategy is a process of iteration and
improvement.
CG DevX comes with minimalistic set of labels / tags as part of integrated labeling strategy.
Main goal is to address immediate needs like lineage tracking, cost attribution, and allow user to grow and evolve the
strategy.

### Cost attribution

Map spending data to the business allowing setback / chargeback to enable:

- Meaningful taxonomy of costs
- Organizational breakdown (by division, business unit, management hierarchy)
- Functional breakdown (by workload)
- By cost center, service, or other unit (map to available taxonomy)

### Secure Operations

By using shared ownership and consumer information associated with resources you could quickly track all the affected
parties in case of security incident. It helps to isolate, identify, and notify the right person quicker, reducing
potential impact.

Such data could include:

- Workload owner name
- Operations team / Team that is accountable for day-to-day operations
- Business criticality / Business impact of the resource or supported workload
- Disaster recovery / Business criticality of the workload or service
- End date of the project / Date when the workload is scheduled for retirement

## Rightsizing and cost recommendations

Containerization helps to solve the rightsizing problem by making workload smaller and easier to properly distribute
across multiple nodes. It's still hard to properly rightsize and attribute workloads. In K8s world rightsizing should be
done on multiple levels.

**Pod / Container Rightsizing**

Ensure that your containers are asking for an appropriate amount of resources. Since asking for too little means your
application is not able to perform, often there is some buffer between the requests or limits configured for a container
and what it really needs. When this buffer is larger than necessary is when there is opportunity for cost savings.
Such opportunities are identified and highlighted by KubeCost. It works with Vertical Pod Autoscaler (VPA) and
Horizontal Pod Autoscaler (HPA), where VPA helps to adjust the requests and limits configuration based on how much a
container is seen to use, and HPA is meant to scale out and in.

**Node Rightsizing**

Is the choice of worker node type for the cluster and bin packing. KubeCost will give you instance type recommendations,
however in practice this could be harder to do. CG DevX allows you to use multiple node types per node group, and
multiple node groups to allow better resource utilization. To enable additional node groups you should update your
hosting module IaC configuration. Please
check [this section](../../operators_guide/platform_management/iac/core_infrastructure_management.md) for mode details.

CG DevX comes with the limits set based on "demo scenario", with a priority on minimizing costs and running core
services with low number of active workloads. On K8s cluster level - Cluster autoscaler is turned on by default. Those
limits should be adjusted for heavy use.
For user workloads we recommend starting with sane limits and adjusting them after some period of time (usually <= 1
week) that allows you to understand load patterns.

## Labels / Tags recommendations

Considerations

- Labels / tags should not be used as a configuration management database.
- In general, values that are not easily human-readable can have a human-readable name/description added another label /
  tag.
- Labels / tags should be (semi-) static.
- Namespaces are used to help organize data specific to domain or organization. Namespace should be either dash `-` or
  dot-separated `:`. Azure doesn't support colon-separator `:`
- kebab-case or camelCase is used for keys. Note: some AWS resources may have issues with kebab-case.
- The format for Multi-value is the following:
  - Single-Value: `name=value`
  - Multi-Value: `name=value1:value2`
  - Multi-Attribute: `name=attribute1=value1/attribute2=value2`
  - Combination: `name=attribute1=value1:value2/attribute2=value1:value3`
- When date-ime data is stored in labels / tags the UTC ISO 8061 standard is used.

### Cloud Resources Labels / Tags

Below are suggested labels with examples:

| Use Case            | Tag                                      | Rationale                                                     | Allowed Values (Listed or value prefix/suﬃx) |
|---------------------|------------------------------------------|---------------------------------------------------------------|----------------------------------------------|
| Cost Allocation     | cg-devx.cost-allocation.workload-id      | Track cost vs value generated by each line of business        | analytics/landing-page                       |
| Cost Allocation     | cg-devx.cost-allocation.business-unit-id | Monitor costs by business unit                                | architecture/devops/finance                  |
| Cost Allocation     | cg-devx.cost-allocation.cost-center      | Monitor costs by cost center                                  | 123-*                                        |
| Cost Allocation     | cg-devx.cost-allocation.owner            | Which budget holder is responsible for this workload          | marketing/retail-support                     |
| Data Classification | cg-devx.data.classification              | Classify data for compliance and governance                   | public/private/confidential, restricted      |
| Security            | cg-devx.security.application-owner       | IT Application Owner                                          | NameA/NameB                                  |
| Security            | cg-devx.security.operations-team         | Team that is accountable for day-to-day operations            | TeamA/TeamB                                  |
| Security            | cg-devx.security.business-criticality    | Business criticality of the application, workload, or service | High/Medium/Low                              |

Business criticality values

| Business criticality | Description                                                                                       |
|----------------------|---------------------------------------------------------------------------------------------------|
| high                 | Exploitation causes serious public image damage and financial loss with long-term business impact |
| medium               | Applications connected to the internet that process financial or other private information        |
| low                  | Typically internal applications with non-critical business impact                                 |

### K8s Cluster Resources Labels

| Use Case              | Label                                 | Rationale                                                                        | Examples                      |
|-----------------------|---------------------------------------|----------------------------------------------------------------------------------|-------------------------------|
| Cost Allocation       | cg-devx.cost-allocation.environment   | Defines SDLC stage                                                               | dev/test/stg/prod             |
| Cost Allocation       | cg-devx.cost-allocation.wl-name       | The name of the workload                                                         | workload1/workload2/workload3 |
| Cost Allocation       | cg-devx.cost-allocation.service       | The name of the service in the workload                                          | service1/service2/service3    |
| Cost Allocation       | cg-devx.cost-allocation.version       | The current version of the application (e.g., a SemVer 1.0, revision hash, etc.) | 5.7.21                        |
| Cost Allocation       | cg-devx.cost-allocation.component     | The component within the architecture                                            | frontend, backend, database   |
| OperationalExcellence | cg-devx.operations.deployed-by        | The controller/user who created this resource                                    | hpotter, ggranger, rweasley   |
| Security              | cg-devx.security.business-criticality | Business criticality of the application, workload, or service                    | High, Medium, Low             |

## CG DevX default labels

### Cloud Resources Labels / Tags

| Tag / Label                                  | Value                | Rationale                                           |
|----------------------------------------------|----------------------|-----------------------------------------------------|
| cg-devx.cost-allocation.workload-name        | <workload name>      | Workload name. Applied to workload resources        |
| cg-devx.cost-allocation.workload-environment | dev/sta/prod         | Workload environment. Applied to workload resources |
| cg-devx.cost-allocation.cost-center          | platform,development | Indicates cost center                               |
| cg-devx.metadata.owner                       | platform-admin       | Indicates team owning the resource                  |
| cg-devx.metadata.service                     | <service_name>       | Indicates service                                   |

### K8s Cluster Resources Labels

| Label                                        | Value                | Rationale                                           |
|----------------------------------------------|----------------------|-----------------------------------------------------|
| cg-devx.cost-allocation.workload-name        | <workload name>      | Workload name. Applied to workload resources        |
| cg-devx.cost-allocation.workload-environment | dev/sta/prod         | Workload environment. Applied to workload resources |
| cg-devx.cost-allocation.cost-center          | platform,development | Indicates cost center                               |
| cg-devx.metadata.owner                       | platform-admin       | Indicates team owning the resource                  |
| cg-devx.metadata.service                     | <service_name>       | Indicates service                                   |

