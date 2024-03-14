# Installation process

Installation should be done from a dedicated machine (local machine or host) that will be running CG DevX CLI (`cgdevxcli`) interactive tool.

With this installation approach infrastructure is handled in terraform, core services are handled in GitOps way,
with ArgoCD.
When the initial installation of ArgoCD is done by CLI, ArgoCD will self-manage its own configuration using GitOps.
Configuration management of some core services is done using terraform capabilities.
While some users may prefer using Ansible or other tools, we find terraform well suited for this purpose.

The main reason behind this approach is
to enable use of the same configuration with different cloud/hosting and git providers,
while retaining the ability to further customize installation without affecting reference implementation.

```mermaid
flowchart LR
    A(CLI on local machine) -->|1 Invokes| B(Terraform)
    B -->|3 Creates| D(Cloud Resources)
    B -->|2 Creates| C(Git repository)
    C -->|5 GitOps reconciliation| E
    A -->|4 Installs| E(ArgoCD)
    E -->|6 Manages| F(Core Components)
```

## Configure Cloud provider

Prepare your cloud account([AWS](./cloud/aws_setup.md), [Azure](./cloud/azure_setup.md)).

## Configure Git Provider

Prepare your Git provider ([GitHub](./vcs/github_setup.md)).

## Get CG DevX CLI

CG DevX CLI simplifies initial setup of CG DevX reference architecture and further management of the platform.
The setup process is intended to be executed from an operator's machine and will create a local folder (`~/.cgdevx`)
containing tools,
local version of GitOps repository, and configuration files.
All subsequent commands should be executed from the same machine, as they will rely on a data created by setup process.

You could allow other users to manage CG DevX installation by transferring state file
(`state.yaml`) and certificates (`cgdevx_ed*`, `cgdevx_k8s*`) from the local folder to another machine.
Please note that this user should have the same access level to K8s cluster as one who provisioned the system.
Additional permissions and configuration changes may be required depending on authentication options
used for cloud provider.

## Installing CG DevX reference architecture

Once installed the CLI,
you should be able to provision a cluster using [`setup`](./cli_commands.md#setup) command.

CG DevX CLI tool will provide detailed information on setup process progress,
and provide you with platform user credentials.
Please store them safely.

Your kubeconfig file will be located in CG DevX local folder (`~/.cgdevx/kubeconfig`)

## Managing Workloads

Once you have a running cluster, you could create your first workload using
this [guide](../workload_management/workloads.md).