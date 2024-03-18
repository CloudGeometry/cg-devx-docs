# CG DevX CLI Workload Management commands

Below is a list of Workload Management commands supported by the CG DevX CLI tool.
Workload Management commands depend on local cluster metadata and:
a) can only be executed when the CG DevX cluster is already provisioned, and
b) should be executed from the same machine where the cluster was provisioned.

For more details on cluster provisioning, please check the [setup command documentation](../installation/cli_commands.md#setup)

## Create

The `create` command generates a declarative configuration of resources required for the workload to function. The configuration is placed in the
platform
GitOps repository. The CLI automatically pushes changes to a feature branch and creates a Pull Request (PR). Changes
introduced with PR should be reviewed and adjusted when necessary.
All the changes to the platform GitOps repository should be applied via Atlantis by typing `atlantis apply` in the PR
comments section.

The `workload create` command creates:

- The configuration for the **VCS** module to create the Workload source code monorepo and Workload GitOps repo
- The configuration for the **Secrets** module to create the Workload secrets namespace and RBAC
- The configuration for the **Core Services** module to create the Image Registry, Code Quality, etc. projects for the Workload
- The ArgoCD dedicated Workload Project ApplicationSet to automatically deliver Workload services

The `workload create` command can be executed using arguments or environment variables.

**Arguments**:

| Name (short, full)                        | Type                                    | Description                                       |
|-------------------------------------------|-----------------------------------------|---------------------------------------------------|
| -wl, --workload-name                      | TEXT                                    | Workload name                                     |
| -wlrn, --workload-repository-name         | TEXT                                    | Name for Workload repository                      |
| -wlgrn, --workload-gitops-repository-name | TEXT                                    | Name for Workload GitOps repository               |
| --verbosity                               | [DEBUG, INFO, WARNING, ERROR, CRITICAL] | Set the logging verbosity level, default CRITICAL |

> **Note!**: For all names use kebab-case.

**Command snippet**

Using command arguments

```bash
cgdevxcli workload create --workload-name your-workload-name \ 
                          --workload-repository-name your-workload-repository-name
                          --workload-gitops-repository-name your-workload-gitops-repository-name
```

## Bootstrap

This command creates the folder structure and injects all the necessary configurations for your Workload into repositories created
by the [create command](#create).

By default, the bootstrap command uses:

- The CG DevX Workload template [repository](https://github.com/CloudGeometry/cg-devx-wl-template)
- The CG DevX Workload GitOps template [repository](https://github.com/CloudGeometry/cg-devx-wl-gitops-template)

For other template options, please see [bootstrap templates](./bootstrap_templates.md)

You can also fork and customize existing template repositories and use them by providing custom template repository URLs
and branches.

> **Note!**: The boostrap command must be executed using the same workload and workload repository names. For example, if your `--workload-name` is `my-workload` your `--workload-repository-name` should be `my-workload-repo`.

The `workload bootstrap` command can be executed using arguments or environment variables.

**Arguments**:

| Name (short, full)                        | Type                                    | Description                                       |
|-------------------------------------------|-----------------------------------------|---------------------------------------------------|
| -wl, --workload-name                      | TEXT                                    | Workload name                                     |
| -wlrn, --workload-repository-name         | TEXT                                    | Name for Workload repository                      |
| -wlgrn, --workload-gitops-repository-name | TEXT                                    | Name for Workload GitOps repository               |
| -wltu, --workload-template-url            | TEXT                                    | Workload repository template                      |
| -wltb, --workload-template-branch         | TEXT                                    | Workload repository template branch               |
| -wlgu, --workload-gitops-template-url     | TEXT                                    | Workload GitOps repository template               |
| -wlgb, --workload-gitops-template-branch  | TEXT                                    | Workload GitOps repository template branch        |
| -wls, --workload-service-name             | TEXT                                    | Workload service name                             |
| -wlsp, --workload-service-port            | NUMBER                                  | Workload service port, default 3000               |
| --verbosity                               | [DEBUG, INFO, WARNING, ERROR, CRITICAL] | Set the logging verbosity level, default CRITICAL |

> **Note!**: For all names, use kebab-case.

**Command snippet**

Using command arguments

```bash
cgdevxcli workload bootstrap --workload-name your-workload-name \ 
                          --workload-repository-name your-workload-repository-name
                          --workload-gitops-repository-name your-workload-gitops-repository-name
                          --workload-service-name your-first-service-name
                          --workload-service-port your-first-service-name-port
```

## Delete

This command removes the declarative configuration of resources required for the workload to function. The CLI automatically pushes changes to a
feature branch and creates a Pull Request (PR). Changes introduced with PR should be reviewed and adjusted when
necessary.
All the changes to platform GitOps repository should be applied via Atlantis by typing `atlantis apply` in the PR
comments section.

The `workload delete` command deletes all the configurations generated by `workload create` [command](#create):

When executed with the `--destroy-resources` flag it will also destroy all the resources created for a specific workload.
Please note that the workload GitOps repository name should match the one for the workload.
When executing with the `--destroy-resources` flag enabled it **must** be executed by the cluster owner.
Under the hood, it will execute `tf destroy` locally, and `tf state` storage is protected and accessible only by the owner. 

> **NOTE!**: This process is irreversible!

The `workload delete` command can be executed using arguments or environment variables.

**Arguments**:

| Name (short, full)                        | Type                                    | Description                                       |
|-------------------------------------------|-----------------------------------------|---------------------------------------------------|
| -wl, --workload-name                      | TEXT                                    | Workload name                                     |
| -wldr, --destroy-resources                | Flag                                    | Destroy workload resources                        |
| -wlgrn, --workload-gitops-repository-name | TEXT                                    | Name for Workload GitOps repository               |
| --verbosity                               | [DEBUG, INFO, WARNING, ERROR, CRITICAL] | Set the logging verbosity level, default CRITICAL |

> **Note!**: For all names, use kebab-case.

**Command snippet**

Using command arguments

```bash
cgdevxcli workload delete --workload-name your-workload-name
```
