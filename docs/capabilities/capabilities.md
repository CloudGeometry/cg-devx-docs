# Capabilities

AKA Core services

![modules](../assets/diagrams.drawio)

In CG DevX, each module is meticulously crafted and encapsulated to function independently within its designated area of usage. This isolation ensures that modules remain self-contained and do not interfere with the functionality of other components. Consequently, any changes or updates made to one module have minimal impact on the overall system, promoting stability and reliability.

**Flexibility and Customizability**:
The modular architecture of CG DevX empowers users to tailor their platform experience to meet their unique requirements. Since modules are decoupled and autonomous, users can easily customize individual components without affecting the integrity of the entire system. This flexibility enables teams to curate a platform that aligns precisely with their specific needs and preferences.

**Seamless Module Replacement**: 
In the dynamic landscape of technology, advancements and innovations are ever-evolving. CG DevX's modular design ensures that users can readily embrace new technologies by seamlessly replacing existing modules with improved alternatives. This empowers teams to stay at the forefront of technological progress without having to undergo cumbersome and disruptive system-wide changes.

**Efficient Troubleshooting and Maintenance**: 
Isolated modules simplify the troubleshooting and maintenance process in CG DevX. If an issue arises within a specific module, it can be addressed independently without necessitating an extensive system-wide investigation. This targeted approach accelerates the resolution of problems, reducing downtime and enhancing the overall platform experience.

### Source Code Management

Source code repositories store your business logic services code, manifests, and Infrastructure as Code
(IaC).
They act as a ledger and provide a historical lineage of changes.
They allow for developers and operators to work collaboratively on common codebases.

CG DevX standardises workflows associated with code review and automation driven by git, AKA "GitOps."
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

Infrastructure as Code (IaC) uses the foundational elements of Infrastructure as a Service (IaaS) to streamline and standardize the deployment of various cloud resources. By offering a programmatic approach to infrastructure management, IaC simplifies the development of systems that are both scalable and adaptable to specific organizational needs.

#### Key Features of IaC at CG DevX:

1. **Unified Template Library**:
   CG DevX provides a comprehensive collection of IaC templates, enabling quick deployment and management of infrastructure across various cloud environments. This library serves as a  resource for both creating new projects and scaling existing applications.

2. **Development of Advanced Resources**:
   Our IaC framework lets teams develop complex, higher-order resources that align closely with business objectives, going beyond basic cloud provider offerings to deliver tailored functionality.

3. **Enforcement of Best Practices**:
   By embedding sensible defaults and strong security protocols into infrastructure configurations, CG DevX makes sure deployments comply with both industry standards and the organization's internal security policies.

#### Approaches to Infrastructure Management:

No two teams are alike, so CG DevX supports multiple reconciliation strategies:

- **Occasional Reconciliation Tools**:
  Platforms such as Terraform and AWS CloudFormation are integrated into our workflow to manage infrastructure with periodic updates, providing stability and control over when changes are applied.

- **Continuous Reconciliation Tools**:
  Tools such as Crossplane are used for environments where continuous updates are crucial. These tools maintain constant synchronization with the desired infrastructure state, ideal for dynamic and rapidly evolving cloud landscapes.

#### Preferred IaC Tools:

- **For Workload IaC**: Terraform and Crossplane are recommended for their robustness and flexibility, supporting both static and dynamic environments respectively.
- **For Platform IaC Management**: Terraform is the primary tool, trusted for its comprehensive feature set and reliability in maintaining consistent infrastructure states across deployments.

#### Integration with Kubernetes:

The use of Kubernetes alongside IaC tools like Crossplane increases our capability to manage resources seamlessly and efficiently. This integration is pivotal in achieving real-time, automated updates and maintaining system integrity across our operational landscape.

Through these strategies and tools, CG DevX is committed to delivering an infrastructure management solution that not only meets the current needs of our teams but also adapts to future demands, ensuring a resilient and scalable cloud infrastructure.

### Continuous Integration (CI)

Continuous integration often refers to the build and unit testing stages of the release process.
With raising complexity of workflow,
it could be also treated as an orchestrator of tasks,
with an end goal of a workflow that makes the application ready for delivery.
And not limited to classic parts of CI, or running tests (unit tests, smoke tests, integration tests, acceptance tests,
etc.), validations, and verifications.

**Added Value**:

- Imperative definitions of steps to be completed
- Uses DSL for describing directional workflow graph
- Enables side effects like notifications or manual interventions

### Continuous Delivery (CD)

The primary aim of Continuous Delivery (CD) is to prepare infrastructure and applications to efficiently handle production-level demands.

#### Adoption of GitOps

GitOps represents an evolving approach within CD, where automation ensures alignment between the intended and actual states of system resources. This connection is maintained through a git repository, which acts as the definitive source for application and infrastructure definitions. Changes to this repository trigger updates via controllers such as ArgoCD and FluxCD, ensuring system consistency. Although ArgoCD and FluxCD are similar, they can operate together, improving both flexibility and coverage.

**Added Value**:

- Automation Benefits: Each successful code merge triggers automated processes to build, test, and release updates, streamlining the deployment cycle.
- Continuous Deployment: With this feature enabled, updates are seamlessly transitioned into live environments, achieving a state of constant readiness and deployment without manual intervention.
- Enhanced Testing: CD supports extensive testing regimes that extend well beyond basic checks, incorporating comprehensive E2E or functional testing, often within preview environments.
- Risk-Managed Deployments: Incorporates methods like Blue/Green or Canary deployments, which introduce new versions alongside the old, reducing rollout risks.
- Feature Management: Utilizes feature flags to manage the exposure of new functionalities, allowing controlled and incremental user exposure.
- Operational Efficiency: Accelerates delivery, reduces time to market, and enhances the feedback loop, leading to quicker identification of issues and adjustments.

### Secrets Management

Secrets Management provides long-term secured storage and management for sensitive data
(passwords, secrets, access keys, certificates, etc.).
Similar to VCS, it can provide versioning and meta-data capabilities,
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
- Values can be extended with meta-data
- Values can be versioned
- Provides tight access control
- May offer the ability to generate/lease/rotate/revoke secret

### Identity and Access

CG DevX offers Identity and Access management as part of the platform.
By unifying and centralizing such a critical functionality,
CG DevX allows other core services to offload proof of identity and access management to the platform.
This improves the maintainability of the system and makes access policies more transparent.

To be self-contained, CG DevX uses Hashicorp Vault OIDC capabilities,
but can be integrated with other providers such as Active Directory, Okta, etc.

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

The Portal is a key developer productivity tool that serves as a high-order interface or umbrella for all the
underlying tooling and services presenting them in a user-friendly manner.
CG DevX integrates with Backstage to provide single pane of glass developer productivity solution.

**Added Value**:

- Software catalog of all components, systems and domains, and their cross-dependencies
- Documentation system using the docs-as-code approach, where docs are typically in Markdown, and stored in code
  repositories.
- Catalog of templates for creating new projects or consuming services.
- Trust and security onboarding automation.

### Observability

Gain deep insights into the health and performance of your platform and workloads.
Log and telemetry data collection and visualization
tooling tracks the health of the system.

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
However, such behavior can be detected
and prevented at early stages by providing additional checks within many services with platform toolbox.
Admission control can also be a common substrate for injecting policy controls or building guardrails within the
platform to meet security or regulatory requirements.

**Added Value**:

- Ensures internal policies are obeyed
- Kubernetes can enable a common policy plane
- Can prevent security issues by detecting issues in the early stages

### Workload Runtime

This is the main platform runtime. It can also be thought of as a deployment target for the applications that make up
the
platform.
Kubernetes is the compute and also the substrate for the foundation of platform capabilities.

### Incident Response and Automation

The goal of incident response is to ensure a quick and efficient resolution to incidents detected by the observability stack.
Usually it also serves as a dedicated communication channel to collaborate on resolving the incident.
It ccan also leverage run-books to automate repetitive tasks and improve incident response times.

**Added Value**:

- Detect, triage, and resolve incidents
- Foster collaboration among teams responsible for incident response

### Cost Management

This component allows transparent resource ownership and lineage tracking,
improving overall cost visibility and enabling cost attribution.
It provides insights on resource usage, and helps to identify cost-saving opportunities, and implement effective
cost-optimization strategies.
Some cost savings opportunities like resource cleanup, and on/off schedules can be automated.
It integrates with governance and enables cloud and Kubernetes resource labeling.

**Added Value**:

- Cost attribution back to a service, team, or consumer of the service
- Rightsizing and cost optimization recommendations
- Visualise spending over time allowing to detect unwanted changes
