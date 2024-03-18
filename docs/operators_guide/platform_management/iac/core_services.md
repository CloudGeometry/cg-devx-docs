# Core platform services

Configuration management of core platform services is done using the `core_services` module.
This module acts as an umbrella, managing the following services:

- **Registry**: 
  - **Harbor** - Image registry
- **Code Quality**: 
  - **SonarQube** - Code quality inspection tool

Configuration is described in `/terraform/core_services/main.tf`.

The set of parameters for the module includes:

- **code_quality_oidc_client_id**: Code Quality service OIDC client id
- **code_quality_oidc_client_secret**: Code Quality service OIDC client secret
- **code_quality_admin_password**: Code Quality service admin user password, auto-generated and stored in Vault
- **registry_oidc_client_id**: Registry services OIDC client id
- **registry_oidc_client_secret**: Registry services OIDC client secret
- **registry_main_robot_password**: Registry services admin user password, auto-generated and stored in Vault
- **workloads**: workloads definitions loaded from the `workloads.tfvars.json` file

The core services module uses workload definitions to enable a personalized experience for each workload team,
isolating workloads using abstraction, for example, a project in each of the services, and limiting access to those abstractions.
Definitions are provided using the `workloads` variable passed via the `terraform.tfvars.json` file.

Below is an example of a `terraform.tfvars.json` file containing one workload called `demo-workload`. <!-- There's no core module described here. -->

```json
{
  "workloads": {
    "demo-workload": {
      "description": "CG DevX Demo-Workload workload definition"
    }
  }
}
```
