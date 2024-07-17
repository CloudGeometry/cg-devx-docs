# Cost Management

Our goal is to improve cost visibility and provide cost savings recommendations for the platform and its core services.
To achieve this, CG DevX reference architecture incorporates cloud and in-cluster resource labeling/tagging, and
integrates with real-time cost visibility and insights tools. By default, the free version
of [KubeCost](https://www.kubecost.com/) is installed with the platform and can be replaced
with [OpenCost](https://www.opencost.io/) or other tools from our integration catalog.

To properly manage costs, users of CG DevX should have a full-scale cost management (visibility, analysis, optimization,
governance) FinOps solution.

> CG DevX reference architecture cannot and should not replace your primary cost management solution.

## Cost Visibility & Attribution

As with many practices in operations, implementing a labeling/tagging strategy is a process of iteration and
improvement. CG DevX comes with a minimalistic set of labels/tags as part of an integrated labeling strategy. The main
goal is to address immediate needs like lineage tracking and cost attribution, and allow users to grow and evolve the
strategy.

### Cost Attribution

Map spending data to the business to enable setback/chargeback by:

- Creating a meaningful taxonomy of costs
- Providing an organizational breakdown (by division, business unit, management hierarchy)
- Offering a functional breakdown (by workload)
- Classifying by cost center, service, or other unit (map to available taxonomy)

### Secure Operations

By using shared ownership and consumer information associated with resources, you can quickly track all affected parties
in case of a security incident. It helps to isolate, identify, and notify the right person quicker, reducing potential
impact.

Such data could include:

- Workload owner name
- Operations team / Team accountable for day-to-day operations
- Business criticality / Business impact of the resource or supported workload
- Disaster recovery / Business criticality of the workload or service
- End date of the project / Date when the workload is scheduled for retirement

## Rightsizing and Cost Recommendations

Containerization helps solve the rightsizing problem by making workloads smaller and easier to properly distribute
across multiple nodes. However, it is still challenging to rightsize and attribute workloads correctly. In the K8s
world, rightsizing should be done on multiple levels.

### Pod/Container Rightsizing

Ensure that your containers request an appropriate amount of resources. Asking for too little means your application
cannot perform, while too much means inefficiency. Often, there is a buffer between the requests or limits configured
for a container and what it really needs. When this buffer is larger than necessary, there is an opportunity for cost
savings. KubeCost identifies and highlights such opportunities. It works with Vertical Pod Autoscaler (VPA) and
Horizontal Pod Autoscaler (HPA), where VPA helps adjust the requests and limits configuration based on actual usage, and
HPA scales out and in.

### Node Rightsizing

Node rightsizing involves choosing the right worker node type for the cluster and bin packing. KubeCost provides
instance type recommendations, but in practice, this can be challenging. CG DevX allows you to use multiple node types
per node group and multiple node groups to improve resource utilization. To enable additional node groups, update your
hosting module IaC configuration. Please
check [this section](../../operators_guide/platform_management/iac/core_infrastructure_management.md) for more details.

CG DevX comes with limits set based on a "demo scenario," prioritizing cost minimization and running core services with
a low number of active workloads. On the K8s cluster level, Cluster Autoscaler is turned on by default. These limits
should be adjusted for heavy use. For user workloads, we recommend starting with reasonable limits and adjusting them
after some time (usually ≤ 1 week) to understand load patterns.

## Labels/Tags Recommendations

### Considerations

- Labels/tags should not be used as a configuration management database.
- Values that are not easily human-readable can have a human-readable name/description added to another label/tag.
- Labels/tags should be (semi-) static.
- Namespaces are used to help organize data specific to a domain or organization. Namespace should be either dash `-` or
  dot-separated `.`. Azure doesn't support colon-separator `:`.
- Kebab-case or camelCase is used for keys. Note: some AWS resources may have issues with kebab-case.
- The format for multi-value is the following:
    - Single-Value: `name=value`
    - Multi-Value: `name=value1:value2`
    - Multi-Attribute: `name=attribute1=value1/attribute2=value2`
    - Combination: `name=attribute1=value1:value2/attribute2=value1:value3`
- When date-time data is stored in labels/tags, the UTC ISO 8061 standard is used.

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

CG DevX applies the following labels by default

### Cloud Resources Labels / Tags

| Tag / Label                         | Value          | Rationale                                |
|-------------------------------------|----------------|------------------------------------------|
| cg-devx.cost-allocation.cost-center | platform       | Indicates cost center                    |
| cg-devx.metadata.owner              | platform-admin | Indicates team owning the resource       |
| cg-devx.metadata.cluster-name       | <cluster_name> | Cluster name provided as input parameter |

### K8s Cluster Resources Labels

| Label                               | Value                         | Rationale                                                                  |
|-------------------------------------|-------------------------------|----------------------------------------------------------------------------|
| cg-devx.cost-allocation.workload    | <workload_name>               | Workload name. Applied to workload resources                               |
| cg-devx.cost-allocation.environment | dev,sta,prod                  | Workload environment. Applied to workload resources only                   |
| cg-devx.cost-allocation.cost-center | platform,development          | Indicates cost center                                                      |
| cg-devx.metadata.owner              | platform-admin/workload-admin | Indicates team owning the resource                                         |
| cg-devx.metadata.service            | <service_name>                | Indicates service, workload only                                           |
| cg-devx.metadata.chart-version      | <helm_chart_version>          | Indicates version of Helm chart used to deploy service, core-services only |
| cg-devx.metadata.version            | <application_version>         | Indicates version of application deployed, core-services only              |

