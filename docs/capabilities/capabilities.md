# Capabilities

AKA Core services

![modules](../assets/diagrams.drawio)

In CG DevX, each module is meticulously crafted and encapsulated to function independently within its designated area of usage. This isolation ensures that modules remain self-contained and do not interfere with the functionality of other components. Consequently, any changes or updates made to one module have minimal impact on the overall system, promoting stability and reliability.

**Flexibility and Customizability**:
The modular architecture of CG DevX empowered users to tailor their platform experience to meet their unique requirements. Since modules are decoupled and autonomous, users can easily customize individual components without affecting the integrity of the entire system. This flexibility enables teams to curate a platform that aligns precisely with their specific needs and preferences.

**Seamless Module Replacement**: 
In the dynamic landscape of technology, advancements and innovations are ever-evolving. CG DevX's modular design ensures that users can readily embrace new technologies by seamlessly replacing existing modules with improved alternatives. This empowers teams to stay at the forefront of technological progress without having to undergo cumbersome and disruptive system-wide changes.

**Efficient Troubleshooting and Maintenance**: 
Isolated modules simplify the troubleshooting and maintenance process in CG DevX. If an issue arises within a specific module, it can be addressed independently without necessitating an extensive system-wide investigation. This targeted approach accelerates the resolution of problems, reducing downtime and enhancing the overall platform experience.



### Source Code Management

Source code repositories is a storage of your business logic services code, manifests, and Infrastructure as Code
(IaC).
They act as ledger and provide historical lineage of changes.
They allow for developers and operators to work collaboratively on common codebases.

CG DevX standardise workflows associated with code review and automation driven by git AKA "GitOps".
So, when managed properly, pull requests (and associated changes) can be used as a "system of record" for change control
and approval in regulatory environments.

CG DevX provides unified repository management via VCS modules.

**Added Value**:

- Allows asynchronous collaboration on codebase, including
    - peer reviews
    - quality gates
    - change request approvals
- Centralized on collaboration workflows
- Provides additional control for peer reviews and change request approvals
- Extends VCS quality gates capabilities

### Infrastructure as Code (IaC)

Infrastructure as Code or IaC, builds upon the Infrastructure as a Service (IaaS) offerings from cloud providers.
IaC offers the ability to build common patterns across a mix of heterogeneous resources, abstarcting away the
inconsistency of underlying cloud provider APIs.
It also enables platform teams to build higher order resources that meet specific business needs.

We see two general approaches supported by tools here:
One with occasional reconciliation, like Terraform, AWS CloudFormation, AWS CDK;
and continuously reconciled, like Crossplane.

While there are pros and cons
associated with both approaches to foster reuse of existing IaC CG DevX aims to support bot ot them for workload IaC.
Our opinionated choice for reference implementation here is Terraform and Crossplane.
For the platform IaC management itself, CG DevX relies on Terraform.

**Added Value**:

- Provides a cohesive library of templates
- Allows for higher order components and services to be built
- Can inject sane defaults
- Can enforce security best practices
- Can be continuously reconciled when used in conjunction with Kubernetes

### Continuous Integration (CI)

Continuous integration often refers to the build and unit testing stages of the release process.
With raising complexity of workflow,
it could be also treated as an orchestrator of tasks,
with an end goal of workflow to make application ready for delivery.
And not limited to classic parts of CI, or running tests (unit tests, smoke tests, integration tests, acceptance tests,
etc.), validations, verifications.

**Added Value**:

- Imperative definitions of steps to be completed
- Uses DSL for describing directional workflow graph
- Enables side effects like notifications or manual interventions

### Continuous Delivery (CD)

Continuous delivery’s ultimate goal is to bring infrastructure and workload services into a production ready state.
[GitOps](https://opengitops.dev/) is a trending approach in continuous delivery.
In short, automation continuously ensures that the desired state matches the perceived state, where git repository
serves as a source of truth.
ArgoCD and FluxCD are two most commonly used implementations of these practices.

CG DevX reference implementation is based on ArgoCD.
This doesn't mean that ArgoCD and FluxCD (or other implementations) are mutually exclusive.
They could work in parallel or in tandem to provide better value.

**Added Value**:

- Automation to build, test and release a new version of workload upon every successful merge
- Allows fully automated deployments AKA "Continuous Deployment"
- Facilitates end to end (E2E) testing that goes beyond simple unit or integration tests. Could be used in conjunction
  with ephemeral environments (AKA preview environments).
- It could be used in conjunction with progressive deployment methods like Blue/Green or Canary
- Can also make use of feature flags to allow for “soft launches” of features and functionality
- Shortening feedback loops
- Reducing time to market for new features

### Secrets Management

Secrets Management provides long-term secured storage and management for sensitive data
(passwords, secrets, access keys, certificates, etc.).
Similar to VCS, it could provide versioning and meta-data capabilities,
combined with stricter access controls and auditing.
The storage is also encrypted, and could integrate with Hardware Security Modules or HSMs.
Secrets Management may also offer the ability to generate, lease, rotate and revoke secrets.

CG DevX reference implementation is bsed on Hashicorp Vault
integrated with cloud-specific services via cloud provider module abstraction, e.g., AWS Secrets Manager, or Azure Key
Vault

**Added Value**:

- Secure and durable storage
- Uses simple to work with key value pairs data structure
- Data is encrypted
- Values could be extended with meta-data
- Values could be versioned
- Provides tight access control
- May offer the ability to generate/lease/rotate/revoke secret

### Identity and Access

CG DevX offers Identity and Access management as part of the platform.
By unifying and centralizing such a critical functionality,
CG DevX allows other core services to offload proof of identity and access management to the platform.
This improves the maintainability of the system and makes access policies more transparent.

To be self-contained, CG DevX uses Hashicorp Vault OIDC capabilities,
but could be integrated with other provides like Active Directory, Okta, etc.

**Added Value**:

- Provides SSO
- Reduces duplication of effort
- Identity can be federated

### Artifact registries

Serves as centralized storage of artifacts produced by CI pipeline.
It could also serve as a proxy registry for common external registries, such as Dockerhub, Quay, and GCR.
This integration with external registries could be bidirectional,
allowing offloading of artifacts to external repositories, contributing to Business Continuity and Disaster Recovery.
It provides integration with static code analysis tools,
allowing a combination of the registry and the packaging mechanism (Docker, Helm) to undergo a secure software supply
chain (SSSC) best practices.

**Added Value**:

- Long-term artifact storage.
- Provides built in Role Based Access Control (RBAC) to limit access to artifacts.
- Versioned and is immutable
- Integrated with static analysis tools to detect known vulnerabilities in artifacts.
- Could be extended with Cryptographic Signing, enabling Consistency and Integrity verification.

### Developer Portal

Developer Portal is a key developer productivity tool that serves as a high-order interface or umbrella for all the
underlying tooling and services presenting them in a user-friendly manner.
CG DevX integrates with Backstage to provide single pane of glass developer productivity solution.

**Added Value**:

- Software catalog of all components, systems and domains, and their cross-dependencies
- Documentation system using the docs-as-code approach. Where docs are typically in Markdown, and stored in code
  repositories.
- Catalog of templates for creating new projects or consuming services.
- Onboarding automation for security and trust.

### Observability

Gain deep insights into the health and performance of your platform and workloads.
The overall well-being of the system is tracked via integration with log and telemetry data collection and visualization
tooling.

CG DevX integrates seamlessly with Prometheus, Loki, and Grafana.

**Added Value**:

- Provides insights on system behavior
- Enables close to real time visibility of system events
- Offers logs and metrics-based alerts

### GitOps and PR automation

CGDevX offers robust CI/CD capabilities to accelerate your software delivery lifecycle. Set up seamless pipelines to
automate building, testing, and deploying your applications. Integrate with popular version control systems like Git,
and choose from a range of deployment strategies such as rolling updates and canary deployments. Effortlessly deliver
your applications with confidence.

### Governance and Validation

While "shift left" empowers developers and gives them almost complete control over their workloads,
with great power comes great responsibility.
Without proper guardrails, it is impossible to protect the platform from human errors in a form of misconfiguration,
wide access policies, misplaced labels, etc.

Platforms can make use of specifications to validate all client interactions with the system.
The end goal is to make suspicious or unwanted interaction of the user with the platform visible to operators.
For instance, Kubernetes offers a built-in policy-based validation engine when working with its APIs.
However, such behavior could be detected
and prevented at early stages by providing additional checks within many services with platform toolbox.
Admission control can also be a common substrate for injecting policy controls or building guardrails within the
platform to meet security or regulatory requirements.

**Added Value**:

- Ensures internal policies are abided
- Kubernetes can enable a common policy plane
- Could prevent security issues by detecting issues on early stages

### Workload Runtime

This is the main platform runtime. It can also be thought of as a deployment target for the applications that make up
the
platform.
Kubernetes is the compute and also the substrate for the foundation of platform capabilities.

**Added Value**:

-

### Incident Response and Automation

Incident response goal is to ensure a quick and efficient resolution of incidents detected by observability stack.
Usually it also serves as a dedicated communication channel to collaborate on incident.
It could also leverage run-books to automate repetitive tasks and improve incident response times.

**Added Value**:

- Detect, triage, and resolve incidents
- Foster collaboration among teams responsible for incident response

### Cost Management

This component allows transparent resource ownership and lineage tracking,
improving overall cost visibility and enabling cost attribution.
It provides insights on resource usage, and helps to identify cost-saving opportunities, and implement effective
cost-optimization strategies.
Some cost savings opportunities like resource cleanup, and on/off schedules could be automated.
It integrates with governance and enables cloud and Kubernetes resource labeling.

**Added Value**:

- Cost attribution back to a service, team, or consumer of the service
- Rightsizing and cost optimization recommendations
- Visualise spending over time allowing to detect unwanted changes
