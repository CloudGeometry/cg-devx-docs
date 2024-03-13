# Approach

CG DevX aims
at solving a common set of problems faced by developers and operators
when working together in a modern cloud native environment
by providing templated and opinionated reference implementation of common processes and integrations.
"Opinion" and choice of core components is based on industry best practices and feedback of our community.
We believe that a set of open source tools that we pre-configure together can and more-over is required
to deliver the top of the line experience.

## Modularity and Extensibility

By splitting each logical phase of service delivery strategy into categories,
we enable setting logical boundaries and defining interface.
This interface allows pluggability and extensibility of the platform,
where components could be added, replaced, or removed without breaking the whole composition.
As an example,
user could pick a technology that suits their CI requirements better switching between GitHub actions,
and Argo Workflow, or even use GitLab Pipelines.
They could also use a combination of tools.
At the same time, by providing opinionated initial choice and configuration of components, we aim
at reducing complexity and providing better user experience,
or to make it simple, pre-selecting the right tools for the job.

## Kubernetes first

In an attempt to simplifying selection, integration,
and operation of tools available within the CNCF landscape and enabling cloud interoperability,
we have a strong dependency on Kubernetes.
Kubernetes became a de-facto standard for running scalable,
cloud native services, and most of the CNCF tools will benefit or are designed specifically to work on Kubernetes.
While platform core services and components depend on Kubernetes, Workloads are not tied to Kubernetes.

## Reference architecture and tools

Our goal is to deliver a reference implementation of most common use cases in a platform engineering space.
This could come in the form of "specifications", "manifests", "templates" or "templating mechanisms",
with the goal of demonstrating a best case scenario and / or technology capabilities.
This enables teams to share components, configurations,
and other types of best practices, while keeping the core components to ones that are the best for the job.
Note that while we put our best effort on providing extensibility, we may and most likely not be able to address all
the specifics.
To cover more use cases,
we rely on commitments and contributions from our community members to provide templates, configurations, or new modules.  

# User Personas

## In-house

CG DevX provides a framework within the following user would collaborate. 

### Developers

Engineers creating business-logic-specific services to be consumed by end-customers.
Experts in traditional programming languages and frameworks with limited interest in infrastructure components outside
the ones in-use for their applications.

### Operators

Engineers responsible for the deployment and management of the infrastructure and platform components and services
that provide the foundation for business services.
Experts in cloud services and core components (compute, storage, network),
Infrastructure as Code (IaC), GitOps, automation and scripting.

### Security Engineers

Experts in evaluating, applying, and enforcing security and compliance best practices.
They partner with operators to build a set of guardrails and practices embedded in the platform.

## Integrators

Experts in pre-packaging multiple components into reusable solutions / services.
Could build additional services on top of existing capabilities provided by the platform.
