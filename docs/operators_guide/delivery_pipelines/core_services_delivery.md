# Delivery Pipelines

The [platform repository](../platform_management/platform_repo.md) provides two flows of platform delivery:
one for IaC changes, and another for K8s manifests of core services.

## IaC pipeline

Often, when using terraform or other IaC tools,
changes to the infrastructure are applied directly from the local machine using the CLI (which we find to be a terrible practice),
or via Infrastructure as Code (IaC), which is better but still lacks collaboration.
CG DevX changes the IaC practice to be more GitOps oriented
by managing (planning and applying) terraform changes using an automation tool called Atlantis.

These changes allow implementing most
[GitOps principles](https://github.com/open-gitops/documents/blob/main/PRINCIPLES.md) as:

- Terraform is declarative
- By storing terraform files in Git, we make them versioned and immutable
- Changes will be tracked and applied automatically

> We need to note that it's not possible to be 100% true to GitOps principles while using terraform or any other tool
> that has a state not stored in Git.

By using GitOps approach and Atlantis, we provide better version history,
enhance collaboration, give better visibility of changes.
By keeping a complete history of the changes, we also make it easier to rollback.
Atlantis also provides a locking mechanism, which prevents possible conflicts when working in a shared environment.

### Automatic Plans With Atlantis

Any change request that includes a .tf file will trigger atlantis to run your terraform plan.
Atlantis will post the plan's result to your PR as a comment.

At that point, you now can review and approve the PR.
The default requirements for a PR to be processed by Atlantis are
to have one approval from anyone who is not the owner of the PR.

You can always run a new terraform plan for your PR by adding a comment that says `atlantis plan`.

### Apply and Merge

To marge the PR, add the comment `atlantis apply`.
This will prompt Atlantis to run `terraform apply`.

The results of the `apply` operation will be added to the PR as comments.

If the `apply` is successful, your change will be automatically merged into `main`, the PR will be closed, the branch deleted, and
the state lock will be removed in Atlantis.

![delivery_iac](../../assets/diagrams.drawio)

## Core Services

CG DevX core services are using GitOps and delivered by the continuous delivery tool ArgoCD.
It manages all core services and workloads across all K8s clusters.
ArgoCD is a straightforward mechanism for managing:

- Helm charts, versions, configuration overrides, etc.
- Plain K8s manifests
- K8s manifests with [Kustomize](https://kustomize.io/)

The configuration for all the services installed on your K8s cluster can be found in the platform GitOps repository under the
path `/gitops-pipelines/delivery/clusters/cc-cluster/core-services`.

Files are organized in a logical sequence and contain definitions for a service source, destination, and configuration
overrides.
Core services are built using an "App of Apps" pattern to form one registry.

As the desired state is described declaratively in the GitOps repository, once you change it, it will be detected by ArgoCD,
which will reconcile the state.
In other words,
any apps in Kubernetes that need to be added or adjusted will automatically sync with the state defined in Git.

For more details, please check the ArgoCD [documentation](https://argo-cd.readthedocs.io/en/stable/).

![delivery_platform](../../assets/diagrams.drawio)
