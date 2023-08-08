# Getting started: Operator

Welcome to the CG DevX platform! As a DevOps engineer, your role is crucial in supporting and maintaining the platform. This guide will help you understand how to get started with CG DevX and provide you with the necessary information to effectively support the platform.

Before you start, ensure you have:

- Access to the CG DevX platform.
- A solid understanding of DevOps principles and practices.
- Familiarity with Infrastructure as Code (IaC) concepts and tools like Terraform.
- Knowledge of Kubernetes and containerization.
- Basic understanding of the CG DevX platform.


#### Your Role as a DevOps Engineer

As a DevOps engineer supporting the CG DevX platform, your responsibilities include:

- **Platform Maintenance**: Ensure the platform is running smoothly and efficiently. This includes monitoring system performance, troubleshooting issues, and performing regular updates and upgrades.

- **Infrastructure Management**: Use IaC tools like Terraform to manage the platform's infrastructure. This involves writing and updating IaC scripts, managing resources, and ensuring infrastructure security and compliance.

- **CI/CD Pipeline Management**: Manage the platform's CI/CD pipelines. This includes setting up and configuring pipelines, troubleshooting pipeline issues, and ensuring fast and reliable deployment of code.

- **Security Management**: Implement and maintain security best practices on the platform. This includes managing access controls, monitoring for security threats, and ensuring compliance with security policies and regulations.

- **Supporting Development Teams**: Assist development teams in using the platform effectively. This includes providing technical support, training teams on platform usage, and helping teams troubleshoot issues.


## Installation

Before you begin the installation, ensure that you have the following prerequisites:

- Access to a supported cloud provider (e.g., AWS, Google Cloud, Azure)
- Administrative privileges to create and manage resources in the cloud environment
- Basic knowledge of cloud services and concepts
- Git client installed on your local machine

#### Cloud accounts preparation

Start by setting up an account with your preferred cloud provider if you haven't done so already:
	- [Prepare a Cloud account](account_setup/cloud_account_setup.md)

#### VCS provider

Next, you will need to set up a Version Control System (VCS) provider. CG DevX supports integration with GitHub, GitLab, and Bitbucket. You will need to create a user account with your chosen VCS provider and generate access keys. 

Here's a general guide on how to do it:

1. **Create a User Account**: Visit the website of your chosen VCS provider (GitHub, GitLab, or Bitbucket) and sign up for a new account if you don't have one already. 

2. **Generate Access Keys**: Once your account is set up, you will need to generate access keys. These keys will be used by CG DevX to interact with your VCS provider. Here's how you can do it for each provider:

    - **GitHub**: Go to Settings -> Developer settings -> Personal access tokens -> Generate new token. Give your token a descriptive name, set the expiration date, select the scopes (or permissions) you want to grant this token and click Generate token.

    - **GitLab**: Go to User settings -> Access Tokens. Choose a name and optional expiry date for the token. Choose the desired scopes. Click the 'Create personal access token' button and save the personal access token somewhere safe. Once you leave or refresh the page, you won’t be able to access it again.

    - **Bitbucket**: Go to Bitbucket settings -> App passwords -> Create app password. Give it a name, select the permissions it needs, and click Create. Bitbucket will show you the password once. Copy it and save it somewhere safe because you won't be able to see it again.

Remember to store your access keys securely, as they provide full access to your VCS account. 

## Platform management (GitOps)

With CG DevX, managing your application configurations and infrastructure as code (IaC) becomes effortless through the integration of GitHub repositories. These repositories serve as a centralized hub for storing and versioning your infrastructure and module configurations.

![Screenshot](img/CGDevX_gitops.png)


#### User management

In the world of Infrastructure as Code (IaC), managing users and their access to resources is a critical aspect of maintaining a secure and efficient system. With CG DevX, we've made this process as seamless and straightforward as possible, allowing you to focus on what matters most - delivering value to your users.

CG DevX employs a combination of Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC) to ensure proper access control within the cluster and when connecting to non-cluster resources. This approach ensures that only authorized users can access and manipulate the resources they need, while preventing unauthorized access to sensitive areas of your infrastructure.

In the context of IaC, user management is handled through the definition and application of policies that dictate who can access what resources and when. These policies are defined in code and applied to the infrastructure, allowing for a high degree of automation and consistency.

With CG DevX, user management is handled through the GitOps approach. This means that all changes to user access and permissions are made through code changes in a Git repository. This approach ensures that all changes are tracked and can be audited, providing a clear history of who made what changes and when.

1. **Define User Access Policies**: Define policies that dictate what resources a user or group of users can access. These policies are defined in code and stored in a Git repository.

2. **Apply Policies**: Apply the policies to your infrastructure using the CG DevX platform. This process is automated, ensuring that policies are consistently applied across your entire infrastructure.

3. **Monitor and Audit**: Continuously monitor and audit user access and activities. Any changes to user access policies are tracked in the Git repository, providing a clear audit trail.

4. **Update Policies**: As your needs change, update your user access policies by making changes to the code in the Git repository. These changes are then automatically applied to your infrastructure.


#### Secrets

In the context of CG DevX, Vault is used to manage user secrets. These secrets are stored in an encrypted format in Vault's storage backend and are accessible only via Vault's API. The secrets are encrypted and decrypted only in Vault's memory, ensuring that they are never written to disk in an unencrypted format.

Here's a high-level overview of how secrets management works in CG DevX:

1. **Creating Secrets**: Secrets are created by platform operators or by automated processes within the platform. When a secret is created, it's written to Vault and stored in an encrypted format.

    ```bash
    vault kv put secret/mysecret myvalue=plaintextvalue
    ```

2. **Accessing Secrets**: When a user or an application needs to access a secret, they make a request to Vault's API. The request must include a valid token, which Vault uses to enforce access controls.

    ```bash
    vault kv get secret/mysecret
    ```

3. **Revoking Secrets**: Vault has built-in mechanisms for automatically revoking secrets after a certain period of time. This ensures that if a secret is compromised, it's only valid for a limited period of time.

4. **Auditing**: All interactions with Vault are logged, providing a detailed audit trail that can be used for forensic analysis in the event of a security incident.


Vault's approach to secrets management provides several benefits from a security and compliance perspective:

- **Encryption**: All secrets are stored in an encrypted format, both at rest and in transit. This protects the secrets from being read if the underlying storage is compromised.

- **Access Control**: Vault uses a detailed access control system, ensuring that only authorized users and applications can access specific secrets.

- **Auditability**: Because all interactions with Vault are logged, it's possible to track who accessed which secrets and when. This is crucial for meeting many compliance requirements.

- **Revocation and Rotation**: Vault supports automatic revocation and rotation of secrets, which can help to limit the damage if a secret is compromised.

By using HashiCorp Vault for secrets management, the CG DevX platform provides a secure, auditable, and automated way to handle secrets in a cloud-native environment. Vault's detailed audit log also provides a record of who is accessing secrets, making it easier to comply with security policies and regulations.

#### Repository management

Managing GitHub repositories for CG DevX is made easy with Terraform. To create additional repositories:

1. Open the `terraform/github/repos.tf` file in your `gitops` repository.

2. Add a new section of Terraform code to define the properties of the new repository. For example:

```hcl
	module "your_repo_name" {
     source             = "./modules/repository"
     visibility         = "private"
     repo_name          = "your-repo-name"
     archive_on_destroy = true
     auto_init          = false
	}
```

3. Customize the properties as needed. Specify the desired `visibility`, provide a unique `repo_name`, and configure any other options required.

4. Save the changes and commit them to the `gitops` repository.

#### Infrastructure

**Step 1: Installing CG DevX Core Components**

Before getting started with CG DevX on your local machine, ensure that you have the necessary prerequisites installed. The following steps outline the installation process:

**macOS (using Homebrew):**

If you are using macOS and have Homebrew installed, you can install the CG DevX CLI by running the following command:

```bash
brew install cgdevx/cli/cgdevx
```

To upgrade an existing CG DevX CLI installation to the latest version, run:

```bash
brew update
brew upgrade cgdevx
```

**For Linux (Ubuntu/Debian):**

If you are using a Debian-based system like Ubuntu, you can install the CG DevX CLI by running the following commands:

```bash
wget https://cli.cgdevx.com/install/cgdevx.deb
sudo dpkg -i cgdevx.deb
```

This will download the Debian package and install the CG DevX CLI.

To upgrade an existing CG DevX CLI installation to the latest version, run:

```bash
wget https://cli.cgdevx.com/install/cgdevx.deb
sudo dpkg -i cgdevx.deb
```


**Step 3:  Create Your New CG DevX Cluster**

To create a new CG DevX cluster, you need to provide a YAML configuration file with the required input parameters. Follow these steps:

**Create Cluster Configuration YAML**: Create a YAML file, e.g., `config.yaml`, and define the configuration for your cluster. Below is an example of a cluster configuration with the required input parameters:

```yaml
apiVersion: cgdevx.io/v1alpha1
kind: Cluster
metadata:
  name: my-cluster
spec:
  email: your-email@example.com
  cloud: aws
  cloud-region: us-west-2
  cluster-name: my-cluster
  git-org: your-github-organization-name
  dns-registrar: my-dns-registrar
  domain-name: your-domain.com
  git-access-token: your-git-access-token
```

Adjust the values in the YAML file according to your specific environment and preferences. You can also include optional parameters if needed.

- `email`: Specify the email address to receive alerts and ownership notifications.
- `cloud`: Specify the cloud provider type for the initial setup (e.g., `aws`, `gcp`, `azure`).
- `cloud-region`: Specify the cloud region where the cluster will be deployed.
- `cluster-name`: Provide a name for your cluster.
- `git-org`: Specify the GitHub organization name associated with the cluster.
- `dns-registrar`: Specify the DNS registrar for the domain configuration.
- `domain-name`: Specify the domain name for the cluster.
- `git-access-token`: Provide a valid Git access token with the necessary permissions.

You can also include additional optional parameters such as `cloud-profile`, `cloud-key`, `cloud-secret`, `gitops-repo-name`, `setup-demo-workload`, `gitops-template-url`, and `gitops-template-branch`. Adjust these parameters based on your specific requirements.


Optional Parameters:

- `cloud-profile`: Specifies the cloud profile to use for cluster deployment (if predefined).
- `cloud-key`, `cloud-secret` Provide cloud credentials directly for authentication with the cloud provider's API.
- `gitops-repo-name`: Name of the GitOps repository for cluster configurations (if using an existing one).
- `setup-demo-workload`: Enable automatic setup of a demo workload on the cluster (for testing).
- `gitops-template-url``, `gitops-template-branch`: URL and branch of the GitOps template for cluster configuration.

**Create the Cluster**: Use the CG DevX CLI to create the cluster by applying the YAML configuration file:

```bash
cgdevx create -f config.yaml
```

This command will create the cluster based on the configuration specified in the YAML file.

**Terminal Output**: After running the cluster creation command, the terminal will display the progress and status of the cluster creation. Pay attention to any error or warning messages that may appear.

**Root Credentials**: To obtain the initial passwords for your cluster, run the following command:

```bash
cgdevx show root-credentials
```

**Connecting to Kubernetes**: To connect to your newly created Kubernetes cluster, execute the following command:

```bash
export KUBECONFIG=~/.cgdevx/kubeconfig
```

**View Cluster Pods**: To list all pods running in your cluster, use the following command:

```bash
kubectl get pods -A
```


**Step 4: Making Terraform Changes**

To make changes to your infrastructure and configurations using Terraform:

1. Open a pull request targeting the relevant Terraform directory within the `gitops` repository.

2. Clearly describe the changes you intend to make in the pull request.

3. CG DevX will automatically generate plans, apply the changes, and provide state locks. The progress of the apply process will be reflected in comments on the pull request.

4. Benefit from a streamlined and auditable change management process. The pull request and associated comments will serve as a transparent changelog of all infrastructure and configuration modifications.

![Screenshot](img/CGDevX_demo_gitops_pull.png)

**Step 5: Argo CD Integration**

CG DevX seamlessly integrates with Argo CD, a powerful GitOps continuous delivery tool designed for Kubernetes. Argo CD simplifies the management of applications across your Kubernetes clusters, providing an efficient way to handle Helm charts, their versions, configuration overrides, and ensuring synchronization with your desired state.

![Screenshot](img/CGDevX_Argo_CD.png)

**Argo CD Applications**

All your application configurations within your Kubernetes cluster can be found in the dedicated gitops repository under the path `/registry/<cluster-name>`. The gitops repository acts as a centralized location for managing your applications and their associated settings.

![Screenshot](img/CGDevX_gitops_registry.png)

These YAML files contain comprehensive details about each application, including its source, destination, and any Helm configuration overrides.


## Workload management

#### GitOps

GitOps is a paradigm that places Git at the heart of building and operating cloud-native applications by using tools that can sync public repositories with the live state of a cluster. It leverages the power of Git version control system and the pull request mechanism to manage infrastructure and application deployments.

In the context of CG DevX, GitOps is used to manage the entire lifecycle of infrastructure and applications. The GitOps process begins with the creation of a Git repository that serves as the single source of truth for the system's desired state. This repository contains all the necessary code and configuration files required to instantiate and manage the infrastructure.

The GitOps boilerplate generation process involves the following steps:

1. **Initialize a Git Repository:** A new Git repository is initialized to store all the necessary IaC files. This repository will serve as the single source of truth for the infrastructure's desired state.

```bash
git init my-infrastructure-repo
cd my-infrastructure-repo
```

2. **Create Infrastructure Files:** The necessary infrastructure files are created in the repository. These files are written in a declarative language (like YAML or JSON) and describe the desired state of the infrastructure. In the context of Kubernetes, these files could be Kubernetes manifests, Helm chart values, or Kustomize configuration files.

```bash
touch deployment.yaml service.yaml ingress.yaml
```

3. **Commit and Push Changes:** Any changes to the infrastructure files are committed and pushed to the Git repository. These changes could be a new feature, a configuration change, or a bug fix.

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

4. **Automated Sync:** A GitOps operator (like ArgoCD or Flux) running in the cluster continuously monitors the Git repository for any changes. If a change is detected, the operator synchronizes the live state of the cluster to match the desired state described in the Git repository.

```bash
argocd app sync my-application
```

5. **Verification:** Once the synchronization is complete, the operator verifies that the live state of the cluster matches the desired state described in the Git repository. If there's a mismatch, the operator logs an error and retries the synchronization.


#### Workload bootstrap

**Step 1: Launching the CGDevX CLI**

To start the process of creating a new workload, open your preferred terminal application and launch the CGDevX CLI. Ensure that you are logged in with your CGDevX account credentials. If you haven't installed the CLI or set it up yet, please refer to the CGDevX documentation for installation instructions.

**Step 2: Running the workload-create Command**

Once you have the CLI up and running, it's time to use the `workload-create` command to create your new workload. The `workload-create` command allows you to define various parameters for your workload, such as the workload name and optional repository details. Here's an example command:

```shell
cgdevx workload-create --workload-name <workload-name> --workloadrepository-name <workloadrepository-name> --workload-gitopsrepository-name <workload-gitopsrepository-name>
```

Make sure to replace `<workload-name>`, `<workloadrepository-name>`, and `<workload-gitopsrepository-name>` with the actual values you want to use. The `--workload-name` parameter specifies the name of your workload, while the `--workloadrepository-name` and `--workload-gitopsrepository-name` parameters are optional and allow you to associate a workload with specific repositories.


**Step 2.1: Providing Additional Input Parameters**

In addition to the command options mentioned above, there are also hidden input parameters that you can use to customize your workload further. These parameters are related to workload templates and GitOps repositories. Although they are hidden, they play an important role in defining the behavior and configuration of your workload. Here are the hidden parameters:

- `--workloadtemplate-url`: Specifies the URL of the workload template.
- `--workloadtemplate-branch`: Specifies the branch of the workload template.
- `--workload-gitopstemplate-url`: Specifies the URL of the GitOps template.
- `--workload-gitopstemplate-branch`: Specifies the branch of the GitOps template.

These hidden parameters allow you to leverage predefined templates and configurations to streamline your workload creation process.

**Step 3: Verifying the Workload Creation**

Once you've executed the `workload-create` command with the required and optional parameters, the CLI will initiate the workload creation process. It will provide feedback and status updates as it progresses. After completion, the CLI will display relevant information about the newly created workload, including its ID, name, and associated repositories (if specified).

**Step 4: Waiting for Provisioning to Complete**

After executing the `workload-create` command, the CGDevX platform will begin provisioning your new workload. The provisioning process involves setting up the necessary infrastructure, resources, and configurations for your workload to run smoothly. The exact duration of the provisioning process may vary depending on various factors.

During this step, it is essential to exercise patience and allow the platform to complete the provisioning. The time required for provisioning depends on the complexity of your workload and the current load on the CGDevX platform. The platform may perform various tasks, such as resource allocation, network configuration, and dependency installation, to ensure your workload is ready for use.

It's important to note that you may encounter a waiting period where manual approval from the platform operator is required. This additional step ensures that the workload creation aligns with the platform's policies and guidelines. The platform operator will review your workload request and take any necessary actions to ensure its compliance and security.

To keep track of the provisioning progress, you can periodically check the status of your workload using the CGDevX CLI or the platform's web interface. Look for indicators or notifications that signal the completion of the provisioning process. Once the provisioning is complete, you can proceed with using and managing your workload as intended.

**Step 5: Bootstrapping Your Workload**

Once the provisioning process for your workload is complete, you can proceed with bootstrapping it using the `workload-bootstrap` command in the CGDevX CLI. This step involves setting up the initial configurations and dependencies required for your workload to function properly.


```shell
cgdevx workload-bootstrap --workload-name <workload-name> --workloadrepository-name <workloadrepository-name> --workload-gitopsrepository-name <workload-gitopsrepository-name>
```

Make sure to replace `<workload-name>`, `<workloadrepository-name>`, and `<workload-gitopsrepository-name>` with the actual values you want to use. The `--workload-name` parameter specifies the name of your workload, while the `--workloadrepository-name` and `--workload-gitopsrepository-name` parameters are optional and allow you to associate a workload with specific repositories.
   
**Step 5.1: Providing Additional Input Parameters**

In addition to the command options mentioned above, there are also hidden input parameters that you can use to customize your workload further. These parameters are related to workload templates and GitOps repositories. Although they are hidden, they play an important role in defining the behavior and configuration of your workload. Here are the hidden parameters:

- `--workloadtemplate-url`: Specifies the URL of the workload template.
- `--workloadtemplate-branch`: Specifies the branch of the workload template.
- `--workload-gitopstemplate-url`: Specifies the URL of the GitOps template.
- `--workload-gitopstemplate-branch`: Specifies the branch of the GitOps template.

These hidden parameters allow you to leverage predefined templates and configurations to streamline your workload creation process.

**Step 6: Wait for the bootstrapping process to complete**

Once the bootstrapping process is finished, your workload will be ready for further customization and deployment. You can now proceed to the next steps to manage and operate your workload effectively.


## Delivery pipelines	

### Platform K8s delivery

### IaC+GitOps

## Monitoring

###Dashboards

###Alerts