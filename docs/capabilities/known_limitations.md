# Known challenges and limitations

There are known challenges and limitations in capabilities provided by CG DevX.
Solving some of them will make the platform more opinionated and experience tailored to a specific case,
which we want to avoid.
This allows us
to draw a fine line on things
that we want to provide with the reference implementation and customization that should be done on top of it.
You could think of it in the most complex cases as a fork of platform, that we don't want to do.
Those customizations could also make further updates of the "fork" hard or even impossible.
This sets a first line in a shared responsibility model between us as builder of CG DevX, and users of CG DevX.

When building an Internal Developer Platform using CG Devx,
there is a second line dividing responsibility between operators (AKA a platform team) and developers,
where some responsibilities could be also shared.
This division of responsibility is organization-specific, and should be defined by the user of the CG DevX.

The list below describes known challenges and our vision on means to manage them, and responsibility.

## Runtime

- **Data Persistence**: Is still challenging even with persistent volumes. We do not recommend running
  non-fault-tolerant stateful components (DBs)
- **Data Security**: We provide encryption at rest and in transit for all CG DevX internal components and underlying
  services plus provide fine-grained RBAC policies, still storing sensitive data in containers or persistent volumes is
  not secure.
- **Data Backup and Recovery**: Currently we do not provide a built-in mechanism for Persistent Volumes (PV) data backup
  and recovery. To ensure that data is not lost in the event of a disaster, you should implement a backup and recovery
  strategy including offloading data to a separate location, and testing recovery procedures regularly.
- **Data Management**: CG DevX internal components like a logging and monitoring system have default lifecycle policies
  built in. Responsibly for a data produced by workload is solely on a user of a platform, so you will need to implement
  data management practices (data lifecycle, archiving data, consistency monitoring, etc.) yourself.
- **Data Compliance**: Responsibly for a data produced by workload is solely on a user of a platform, so you may need to
  implement specific data privacy and security measures, including data masking, data retention policies, or data
  sovereignty.

## VCS

- **Collaboration and Branching strategy**: Working in a distributed team is not possible without a proper VCS.
  While changes done to different parts of the system are quite easy to manage,
  when those changes are linked or depend on each other and done by different teams,
  it becomes really hard to track and integrate them without proper conventions.
  We expect teams using GitFlow approach for all the IaC code, and GitOps manifests
  (GitOps repositories) and do a cross-team code review for such changes.
  We strongly suggest using GitFlow for workload codebase as it's already supported by the platform,
  but teams could also use Trunk-based development approach
    - **Versioning**: tracking versions, especially when other services depend on your specific package could be quite
      challenging and could become messy. To simplify a standardised versioning process, we utilise semantic
      versioning and force it usage for deployments and Helm charts and strongly recommend using it for your workloads.
    - **History**: Version Control System (VCS) should provide a clean and human-readable history of changes. To enable
      this, all the developers should follow the same convention when working with the source code. We strongly
      recommend following Conventional Commits convention for commit messages.
    - **Changelog**: VCS should provide users a detailed view of changes associated with a specific version of
      artifacts, which enables developers to quickly identify potential issues associated with change. With usage of
      Conventional Commits, Changelogs could be generated automatically.

## IaC

- **Security & Compliance requirements**: Infrastructure is a matter of compliance fit. Thoughtful implementation of IaC
  enables security and compliance checks against code to detect potential issues faster. CG DevX provides a mechanism of
  policy/compliance as code checks according to security best practices.
  It’s a user's responsibility to implement
  specific security and compliance checks using provided mechanisms.
- **Infrastructure Ownership**: Infrastructure is a core part of Business Service. CG DevX platform is a core
  infrastructure component holding multiple critical business products. Shift-left approach sets clear assignment of
  infrastructure ownership, where operators are responsible for core platform services and development teams - for
  workloads and other cloud services provisioned via the platform those services depend on.
- **Integration with existing resources**: Customers may or may not have IaC used. Furthermore, if IaC is used, it may
  have various forms - Terraform, Crossplane, Pulumi, CloudFormation, etc. To be fully integrated with the platform when
  onboarding a workload, existing resources should be either imported into terraform (or Crossplane for Workloads) or
  recreated.
  CG DevX provides inventory of all resources and services in the form of remote terraform state
  that could be consumed by other tools.
- **Deployment into existing infrastructure**: Some customers may already have existing Kubernetes cluster(s). While
  it’s technically possible to install CG DevX components onto existing cluster, when doing so, we could not
  guarantee integrity of the platform and uncompromised function of its core features, especially in DevSecOps domain,
  thus decided not to support such a scenario.

## IaC Automation

- **Identifying and fixing security issues**: Security misconfigurations as #1 type of incident as per-security reports.
  In a pipeline, the earlier you perform security scanning and testing; the more freedom operators and developers will
  have to accommodate all security feedback. With CG DevX all the necessary checks are built into the pipeline, so
  security is an integral part of your code, not an add-on feature.
  Configuration of those features is up to a user of CG DevX
- **Access management**: In cloud native deployments compared to classic ones, access management has higher associated
  security risks. Integrating the necessary security checks into a delivery pipeline will improve overall security and
  simplify migration towards zero trust. With CG DevX IAM policies (role bindings and policy metadata) are written down
  as code and managed to use the same process as the reset of the infrastructure. Access to infrastructure could be
  restricted or reduced to read-only access as all the changes are applied using change management automation. Changes
  sign-off is done by peer review reducing security risks.
- **Audit logs**: Audit trails are a big part of SOC2, HIPPA, and PCI compliance. There is a misconception that it could
  only be achieved by using specialised systems. When implemented correctly, your VCS change log could be leveraged as
  an audit trail.
  CG DevX IaC PR automation provides a full change log as part of VCS history that could also
  be offloaded to other storages.
- **Upgrades/migrations of stateful infrastructure components**: Stateful components, especially data stores and
  databases vs GitOps and immutable infrastructure.
  Given all pros and cons of approaches, with CG DevX we mostly rely
  on immutable infrastructure to get better repeatability, reduce chances of configuration drift, and have
  versioning with change history.
  We only support in place updates and configuration management for K8s cluster upgrades.
  We strongly recommend
  that you use managed cloud resources where all the maintenance and update procedures are done by cloud
  provider.
- **Cross-components dependencies**: Modern cloud native applications look like a web of interdependent services,
  especially when adopting microservices architecture. On top of that, you have cloud native services and external 3rd
  party services, like payment gateways, your application depends on. When any of these dependencies fails, it can have
  cascading impacts on the rest of your services and on the application as a whole. With CG DevX, your dependencies on
  cloud services are documented as IaC and tagged properly. It’s your responsibility as a user to keep track of your
  application dependencies. CG DevX could be extended by Cloud Assets Catalog, as it's not a part of standard
  installation.
- **Drift detection**: Drift refers to the deviation of actual infrastructure from its intended or expected state. This
  can occur due to various reasons, such as manual configuration changes, human error, etc. Drift can result in
  configuration inconsistencies, security vulnerabilities, and performance issues, making it essential to detect and
  correct it as soon as possible. CG DevX could detect drift for resources provisioned via platform IaC and highlight it
  in special reports using tools that scan and analyze the infrastructure. While we include resources not provisioned
  via IaC as part of those reports, we could not detect changes unless resources are imported to and managed by platform
  IaC solution.

## Workload CI

- **Feature Velocity Issues**: Multiple factors could contribute to delays in release cycles. It could happen due to
  lack of automation, code quality, team motivation, etc. CG DevX provides a fully automated delivery pipeline and
  provides a detailed view on Lead Time for Changes and Deployment Frequency as part of DORA key metrics.
- **Flawed Automated Testing**: Automated tests with no automation are being spoiled over time. They must be
  incorporated into CI pipelines and closely monitored.
  CG DevX provides a well-defined extension points in CI pipelines to integrate quality gates such as automated test.
  Code coverage metric is tracked using integration with a Static Code Analysis component.
  It’s your responsibility as user to track Functional coverage of your tests. Our default CI pipeline has predefined
  quality gate rules based on code coverage and percentage of passed test.
- **Security Vulnerabilities**: Vulnerabilities in contrast to bugs are usually unseen unless they were exposed.
  Shifting security to the left is the first and foremost priority. With CG DevX all the necessary checks are built into
  the pipeline, so security is an integral part of your code, not an add-on feature. We strongly recommend taking a step
  further and using Vulnerability Scanner plugins for IDE, like one for Trivy from Aqua Security.
- **CI process visibility**: CI may generate excessive amount of logs that have little to none business value. Therefore,
  specific metrics (KPIs) and events must be identified and agreed on, and this list should be reviewed with each change
  to the pipeline. CG DevX has a predefined set of metrics and events collected by the system out of the box to enable
  DORA key metrics.
- **Collaboration process**: proper branching strategy and naming conventions are the prerequisites for successful CI
  implementation. Lack of those attributes increases mess in a software delivery process and sometimes becomes detrimental
  rather than helpful. To simplify and standardise the process, we recommend using GitFlow, semantic versioning, and
  conventional commits.
- **Integration with Other Tools**: Automation requires explicit flows across the sequence of tools in the software
  development lifecycle to ensure actionable feedback and improvements. CG DevX CI pipeline consists of pre-defined
  building blocks and provides clear and well-defined extension points.

## Workload CD

- **Multi-tenancy**: In a K8s context, multi-tenancy is when multiple teams are working on different projects in
  the same environment. With highly configurable access control CG DevX will restrict deployments to a
  specific namespace and user access to a specific pipelines. RBAC configurations are automatically generated when
  provisioning new workloads via CG DevX CLI.
- **Monitoring and Alerting**: Out of the box CD metrics such as application reconciliation performance, the controller
  queue depth, and the number of application sync operations will be available via CG DevX observability.
- **Poor communication across teams**: Situation when teams are relying on tribal knowledge of deployment details and
  dates could pose a serious threat to the quality of the service. With CG DevX we try to solve this by providing high
  degree of automation and transparency of delivery pipelines.
  Adoption of proper change management and notification mechanism is under responsibility of the user of the platform.
- **Configuration drift**: Configuration differences between the target cluster of a deployment pipeline can cause it to
  fail. CG DevX CD module could detect configuration drifts. While out of the box security policy will not allow users
  to change configuration directly in cluster its user responsibility to make sure those policies are not altered. CD
  module could auto-sync changes done directly in cluster back to GitOps repository, but we strongly suggest not to use
  this option as it will reduce visibility and trackability of changes.
- **Repository management**: According to many best practices application code, Kubernetes manifests, and IaC code
  should be separated as it simplifies management, helps keep a clean audit trail, and control access. There is no one
  size fits all approach here. To compromise between all pros and cons we provide two repositories per workload, one for
  application source code, and another for Kubernetes manifests, Helm charts, and IaC.

## Workload Code Quality

- **No common code quality standards**:
  To produce a clean and maintainable code organization should understand the importance
  of and adopt code quality standards.
  CG DevX Code Quality module could facilitate standard adoption, but full
  responsibility lies on organization.
- **Code Quality**: Each programming language has its own definition of code quality. In some cases those definitions
  should be adjusted or extended. CG DevX Code Quality ships with predefined code quality rules based on industry
  standards and best practices for each supported programming language. While there is a default one, developers should
  pick the set of rules they are going to use.
- **Quality Gates**: Quality Gates define a set of conditions to be met for code quality to be considered sufficient. We
  suggest using Quality Gate that mandate that all new code must include at least 80% test coverage, and no diagnosed
  security issues and provide a default configuration with that rules. We do not enforce this Quality Gate and
  Developers should decide what exact rules and values they are going to use.
- **False-Positives**: Not all analysis rules are equally suitable in each situation and could produce false positives.
  Lots of them may overwhelm developers making it difficult to focus on other more important fixes. Each time should
  adjust default code quality rules to reduce number of false positives.
- **Unit test coverage reports**: Test coverage information could greatly improve understanding of code quality. CG DevX
  Code Quality module does not generate the coverage report itself. User must set up a language-specific tool to
  calculate and produce a code coverage report as part of delivery pipeline. We do provide configuration samples for
  GitHub and Argo Workflows that could be reused.

## Artifact management

- **Ownership and lineage tracking**: To enable cost tracking and simplify the process of incident response, all the
  resources should have strict assigned owners, and other associated metadata. CG DevX Artifacts management stores
  artifact metadata such as build history, dependencies, readme, and other user-defined information. User is responsible
  for setting all the necessary metadata in addition to one set by default and required by pipeline to operate.
- **Lifecycle management**: CI/CD pipeline could produce dozens of artifacts that could require significant amount of
  storage. Artifacts lifecycle management and storage quotas should be implemented to keep storage under control and
  comply with organizational standards. We provide an automated clean-up process and easy way to set storage quotas per
  workload.
- **Vulnerability Scanning**: It’s important to have a multi-layer vulnerability detection system and check artifacts on
  each step of the delivery pipeline. We provide static analysis of vulnerabilities with Trivy out of the box, and it’s
  possible to connect additional scanners if required by compliance.
- **Disaster Recovery**: To enable recovery of the system in case of region failure, all the artifacts should be
  offloaded to a different region. With CG DevX Artifacts management, artifacts could be replicated to different
  artifact stores in different locations.
- **Public registry request limits and latency**: Certain artifacts must always be available for production environment
  to pull them. Latency of downloading artifacts and public registry rate limits could affect production RTO. CG DevX
  Artifacts management could serve as local cache for publicly available artifacts. This also covers a scenario when
  public registry becomes not available.

## DevSecOps

- **Issues overload**: High volumes of potential vulnerabilities and misconfigurations make it difficult to focus on the
  most important issues. Without risk-based prioritization, developers might spend time on issues that might not even
  represent risk to the organization. CG DevX DevSecOps modules could provide priority for some issues, but them
  should only be considered as recommendations.
- **Balancing speed and security**: Modern development is all about speed and agility, and every team needs to keep the
  pace. While CG DevX provides a security foundation that is agile, adaptable, and fast it could not cover all the
  potential risks and standards. User is responsible for building those additional capabilities on top of existing
  foundation.
- **Lack of resources and knowledge gap**: Most of the organizations lack adequate working knowledge of DevSecOps practices.
  With limited resources bridging this gap becomes even more difficult. CG DevX provides a common platform for operators
  and developers to collaborate with security teams or security consultants. While we provide implementation of security
  best practices, it will not be able to fully compensate for knowledge gaps.
- **Roles and responsibility alignment**: It is challenging to align roles and responsibilities in dynamic environments.
  CG DevX follows shift-left approach where developers and operator are responsible for security of their workloads and
  the platform, and provides a set of measures to give security feedback at all stages of delivery pipeline and as early
  as possible.
- **Workload cloud services dependency**: Cloud services have high complexity associated with them, especially when
  integrating with security tools that were not designed to be cloud native. As CG DevX DevSecOps primary focus is on
  workload code, its dependencies, container runtime, and associated manifests it could not provide full coverage for all
  workload-specific cloud native services, especially when workload relies on many of them.

## Monitoring

- **Complexity**: IDP is a system of services is considered as Complex System and is composed of a large number of
  simple parts. There are a significant amount of interconnections and interactions between parts and at different space
  and time scales. Whole platform and workloads become adaptive and respond to changes in the environment as well as the
  system itself. With lots of dynamic monitoring solution should adopt to those changes and continuously track the state
  of the whole system. CG DevX Monitoring module provides an unified way of metric collection, automated services
  discovery, and health checks that are automatically applied when platform scales in and out.
- **Clustered Environment**: When platform and workload components are located at different places (services, compute
  nodes, etc.) and packed together, monitoring becomes a challenge. It becomes critical to properly track and attribute
  resource usage as one application could cause failures in other, and operators and developers should be able to
  quickly identify and resolve the root cause. CG DevX Monitoring module provides an easy way to zoom in into specific
  cluster/node/namespace/pod helping you to identifying “noisy neighbours”.
- **Data volume and retention periods**: When unmanaged data tend to grow and consume a tremendous amount of resources to
  store, process, and be searchable. Reducing number of metrics you collect and process makes it easier to manage and
  analyze. CG DevX collect only the most critical metrics to reduce data volume, also metrics of internal components have 14
  days retention period by default. When introducing new metrics use of the platform should understand and be
  accountable for thad additional resources required to process new data.
- **Shared Services**: Sharing services enables optimum utilization of resources. On the other hand monitoring shared
  applications is quite challenging. The risk lies in the fact that more consumption of resources by one application
  would affect the performance of other applications. This is especially true for multi tenant applications. CG DevX
  provides continuous monitoring and labeling for all the services. For shared services as part of workloads users are
  required to properly configure monitoring dashboards and labeling so that they could track and attribute usage to a
  specific workload.
- **Monitor workload resources**: Most of the workloads have dependencies on other cloud resources that due to reliability,
  availability, or compliance reasons could not be running on Kubernetes based runtime. CG DevX Monitoring module
  automatically collects metrics exposed by cloud native services provisioned by CG DevX IaC module when they are
  provisioned using cloud native monitoring mechanisms provided by cloud provider and forwarding all the metrics into
  Monitoring module. It also creates a minimal set of alarms as backup circuit for monitoring pipeline running within
  the platform. When resources are not provisioned via CG DevX IaC user takes full responsibility for monitoring them by
  using cloud native, or third party monitoring solutions or plugging them into CG DevX Monitoring
- **Workload rightsizing**: Process of matching actual workload resource requirement with estimations is really hard
  especially for new applications. Also, requirements tend to change over time. CG DevX provides detailed utilization
  dashboard on cluster, cluster node, workload (Kubernetes namespace), service within the workload (kubernetes pod)
  levels and recommendations on resource rightsizing and associated cost savings. More detailed cost visibility
  dashboards and cost-saving recommendations could be provided by CG DevX Cost Visibility.

## Log Management

- **Sensitive data**: It's vital to obfuscate PII and other sensitive data in your logs because you can ensure
  confidentiality and integrity of the data, while continuing to use logs to analyze system behavior and troubleshoot
  issues. You can mitigate the risk of data breaches and unauthorized access and still get your ongoing debugging work
  done. We remove sensitive data such as secrets from delivery pipeline logs. Users of the platform should implement
  additional log obfuscation rules and mechanisms for their workloads.
- **Balance security / usability / log size**: To make sure the logs remain a valuable tool for monitoring system
  performance and troubleshooting, it’s crucial to balance security/usability and log size, and in most cases you need
  to compromise. We remove all non-critical and sensitive data from CG DevX core component logs. Users of the platform
  are responsible for defining amount of logs they want to store and implementing additional log obfuscation rules for
  their workloads.
- **Log size and retention periods**: When unmanaged logs data tend to grow and consume a tremendous amount of resources
  to store, process, and be searchable. In some cases compliance requirements are driving factor behind high retention
  periods. Reducing log sizes, making them easier to manage and analyze. Making logs non-index-able by moving them to
  cold storage could save resources and allow complying with store time requirements. Log data of CG DevX internal
  components has 14 days retention period by default. Out of the box we do not provide a mechanism to offload data to
  cold storage, such functionality could be easily added on top of existing log management pipeline.
- **Collecting logs for workload resources provisioned via IaC**: To simplify troubleshooting all the logs should be
  stored and processed in uniform manner. When new cloud services are provisioned using CG DevX IaC, we will
  automatically configure cloud native logging, and ingest those logs.

## Incident response

- **Lack of context about the incident**: When the incident lacks the contextual information, response team struggles to
  understand the full scale of the problem, make the initial diagnosis, assess the priority, and communicate to the
  other responders, management, and customers. CG DevX provides environment, workload, service ownership information,
  infrastructure and workload metrics and logs. Users of the platform could extend that information by applying
  additional tags on resources and services that will be available during incident. Extended metadata could also be
  provided with Cloud Asset Catalog that could be additionally implemented alongside CG DevX platform installation.
- **Lack of prioritization**: Organization operate with limited resources. Lack of a prioritization scheme can cause
  response teams to spend most of their time on alerts that may not involve any threat depleting those resources. With
  reference implementation of the process all the incidents in CG DevX have the highest priority, and an incident response team
  should prioritise incidents as they process them. Users of the platform could set a default priority based on an incident
  type or source, or other meta-information to simplify their workflow and reduce number of priority one incidents.
- **Tools for communication and escalation**: When a major incident occurs, you need to communicate quickly and
  effectively as success depends on getting the involvement of the right people at the right time. CG DevX provides
  out-of-the-box integration with a paging system, allowing to effectively patch in the right people when they are
  required. You could also notify people using a preferred communication channel via event notification integrations
  using emails, or Slack chat. 
