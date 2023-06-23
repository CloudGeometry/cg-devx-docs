#Getting started

Welcome to the CGDevX Installation Guide! This guide will walk you through the process of installing CGDevX, a powerful platform that simplifies application management in Cloud Native and Cloud Agnostic environments. By following these steps, you'll be able to set up CGDevX and leverage its capabilities to build, deploy, and operate your services.

## Introduction

CGDevX simplifies the management of Cloud Native and Cloud Agnostic runtime environments. By following the CGDevX Golden Path, you can optimize your application development and deployment processes, enhance observability, ensure compliance, and embrace DevSecOps practices.

## Prerequisites

Before you begin the installation, ensure that you have the following prerequisites:

- Access to a supported cloud provider (e.g., AWS, Google Cloud, Azure)
- Administrative privileges to create and manage resources in the cloud environment
- Basic knowledge of cloud services and concepts
- Git client installed on your local machine

## Step 1: Prerequisites for the Infrastructure

1. Start by setting up an account with your preferred cloud provider if you haven't done so already:
	- [Prepare an AWS account](account_setup/aws_account_setup.md)

## Step 2: Installing CGDevX Core Components


Before getting started with CGDevX on your local machine, ensure that you have the necessary prerequisites installed. The following steps outline the installation process:

### macOS (using Homebrew)

If you are using macOS and have Homebrew installed, you can install the CGDevX CLI by running the following command:

```bash
brew install cgdevx/cli/cgdevx
```

To upgrade an existing CGDevX CLI installation to the latest version, run:

```bash
brew update
brew upgrade cgdevx
```

### Other Operating Systems

For installation on different operating systems, architectures, or containerized environments, please refer to the [CGDevX Installation README]() for detailed instructions.

## Step 3:  Create Your New CGDevX Cluster

To create a new CGDevX cluster, you need to provide a YAML configuration file with the required input parameters. Follow these steps:

**Create Cluster Configuration YAML**: Create a YAML file, e.g., `config.yaml`, and define the configuration for your cluster. Below is an example of a cluster configuration with the required input parameters:

```yaml
apiVersion: cgdevx.io/v1alpha1
kind: Cluster
metadata:
  name: my-cluster
spec:
  email: your-email@example.com
  cloud: aws
  cloudRegion: us-west-2
  clusterName: my-cluster
  gitOrg: your-github-organization-name
  dnsRegistrar: my-dns-registrar
  domainName: your-domain.com
  gitAccessToken: your-git-access-token
```

Adjust the values in the YAML file according to your specific environment and preferences. You can also include optional parameters if needed.

- `email`: Specify the email address to receive alerts and ownership notifications.
- `cloud`: Specify the cloud provider type for the initial setup (e.g., `aws`, `gcp`, `azure`).
- `cloudRegion`: Specify the cloud region where the cluster will be deployed.
- `clusterName`: Provide a name for your cluster.
- `gitOrg`: Specify the GitHub organization name associated with the cluster.
- `dnsRegistrar`: Specify the DNS registrar for the domain configuration.
- `domainName`: Specify the domain name for the cluster.
- `gitAccessToken`: Provide a valid Git access token with the necessary permissions.

You can also include additional optional parameters such as `cloudProfile`, `cloudKey`, `cloudSecret`, `gitopsRepoName`, `setupDemoWorkload`, `gitopsTemplateURL`, and `gitopsTemplateBranch`. Adjust these parameters based on your specific requirements.

**Create the Cluster**: Use the CGDevX CLI to create the cluster by applying the YAML configuration file:

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

By following these steps and providing the required input parameters in the YAML configuration file, you can create a new CGDevX cluster and begin managing your applications and workloads.