# Bootstrap templates

CG DevX provides a set of default bootstrap templates for workloads, which are set as a default value
for the [workload bootstrap command](./cli_commands.md#bootstrap).

Those templates provide a user with a pre-defined logical structure of repositories
and allow seamless integration with platform core services, like delivery pipelines (AKA CI/CD), IaC PR automation, etc.

Templates can be cloned and customized by the user and used for bootstrapping new workloads.

The default templates are

- [Workload code repository](https://github.com/CloudGeometry/cg-devx-wl-template)
- [Workload GitOps repository](https://github.com/CloudGeometry/cg-devx-wl-gitops-template)

Those templates provide you with:

- The Workload repository structure
- A Pre-defined Docker file
- CI process
- CD process
- Release promotion process
- GitOps style environment definition
- IaC for out of the cluster cloud resources

Additional templates will be provided later.

## Generic Workload template

### Code repository

The Workload code repository uses the following structure:

```
.
├── .argo
│   ├── build-wf.yaml
│   ├── crane-wf.yaml
│   ├── version-changer-wf.yaml
│   └── wl-service-wf.yaml
├── .github
│   └── workflows
│       ├── multi_service_image_build.yml
│       └── multi_service_parallel_build.yml
└── <service folder>
    ├── Dockerfile
    └── src
        └── <your service code>

```

The repository already contains pre-configured GitHub Actions (in the `.github` folder) and Argo Workflows (in
the `.argo` folder)
to build new images and update workload manifests.

The service folder will be named after the workload.

When using a monorepo for multiple services,
each service should have its own folder and its own Dockerfile.

### GitOps repository

The Workload GitOps repository is using the following structure:

```
.
├── .argo
│   └── promote-wf.yaml
├── .github
│   ├── terraform.yaml
│   └── workflows
│       ├── promote.yaml
│       └── promote_gha_only.yaml_gha
├── atlantis.yaml
├── gitops
│   ├── .gitignore
│   └── environments
│       ├── README.md
│       ├── base
│       │   ...
│       ├── envs
│       │   ├── dev
│       │   │   ...
│       │   ├── prod
│       │   │   ...
│       │   └── sta
│       │       ...
│       └── variants
│           ├── dev
│           │   ...
│           ├── prod
│           │   ...
│           └── uat
│               ...
└── terraform
    ├── .gitignore
    ├── infrastructure
    │   ├── README.md
    │   ├── main.tf
    │   ├── outputs.tf
    │   ├── samples
    │   │   ...
    │   └── variables.tf
    └── secrets
        ├── README.md
        ├── main.tf
        ├── outputs.tf
        ├── secrets.tf
        └── variables.tf
```

The repository already contains pre-configured GitHub Actions (in the `.github` folder) and Argo Workflows (in
the `.argo` folder)
required for GitOps-style workload version promotion.
