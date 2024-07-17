# Core Initiative

CG DEVX leverages a selection of the best open-source tools, reflecting industry best practices and community feedback,
aiming to elevate the overall user experience and streamline the collaborative environment for cloud-native operations
by deploying a standard yet adaptable framework.

We structure our platform around distinct phases of service delivery, each with defined interfaces that support easy
customization and scalability. This enables users to easily tailor their environment by integrating, modifying, or
replacing components without compromising the platform's integrity. For example, users can select from various CI tools,
such as GitHub Actions, Argo Workflow, or GitLab Pipelines, according to their specific requirements. This design
principle helps simplify initial setups and optimize user interaction by pre-selecting the most effective tools for
specific tasks.

CG DEVX predominantly relies on Kubernetes due to its widespread adoption as a scalable solution for cloud-native
services. It forms the backbone of our architecture, ensuring compatibility and enhancing performance with numerous
CNCF-endorsed tools. Note that while Kubernetes acts as a substrate for our core services, those services can manage
workloads outside the cluster, on various platforms. This gives developers greater flexibility to choose different cloud
ecosystems.

CG DEVX provides an implementation blueprint that addresses common platform engineering challenges. This blueprint
includes detailed specifications and templates that serve as demonstrations of the best possible configurations and
technological capabilities. By promoting the exchange of configurations and best practices, we aim to set a high
standard for operational excellence. We recognize the need for ongoing enhancements and adaptations, and we look to our
community to help us extend and refine our platform to suit a diverse array of use cases.

## Stakeholder Personas

### Development Teams

- **Developers:** Specialize in developing application-specific services intended for end users, focusing predominantly
  on software development rather than infrastructure complexities.
- **Operators:** Handle the operational management of platform and infrastructure services. They are responsible for
  cloud-based technologies, Infrastructure as Code (IaC), GitOps, and automated systems.
- **Security Engineers:** Focus on integrating and upholding security protocols within the platform, collaborating
  closely with operators to ensure robust and compliant infrastructure.

### Solution Architects

- **Integrators:** Skilled in combining various components into integrated solutions. These professionals improve the
  platform's value by constructing additional services that capitalize on the pre-established infrastructure.
