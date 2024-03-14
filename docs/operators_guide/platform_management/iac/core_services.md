# Core platform services

Configuration management of core platform services is done using core_services module.
This module acts as an umbrella, managing the following services:

- **Registry**: 
  - **Harbor** - Image registry
- **Code Quality**: 
  - **SonarQube** - Code quality inspection tool

Configuration is described in `/terraform/core_services/main.tf`.

Set of parameters for the module is the following:

- **code_quality_oidc_client_id**: Code Quality service OIDC client id;
- **code_quality_oidc_client_secret**: Code Quality service OIDC client secret;
- **code_quality_admin_password**: Code Quality service admin user password, auto-generated and stored in Vault;
- **registry_oidc_client_id**: Registry services OIDC client id;
- **registry_oidc_client_secret**: Registry services OIDC client secret;
- **registry_main_robot_password**: Registry services admin user password, auto-generated and stored in Vault;
- **workloads**: workloads definitions loaded from workloads.tfvars.json file.

Where

Core services module uses workload definitions to enable personalized experience for each workload team,
isolating workload using abstraction, e.g., project in each of the services, and limiting access to those abstractions.
Definitions are provided using `workloads` variable passed via `terraform.tfvars.json` file.

Below is an example of `terraform.tfvars.json` file containing one workload called `demo-workload`.

```json
{
  "workloads": {
    "demo-workload": {
      "description": "CG DevX Demo-Workload workload definition"
    }
  }
}
```