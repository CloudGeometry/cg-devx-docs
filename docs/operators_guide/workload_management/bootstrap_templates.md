# Bootstrap templates

CG DevX provides a set pf default bootstrap templates for workloads, which a set as a default value
for [workload bootstrap command](./cli_commands.md#bootstrap).

Those templates provide a user with pre-defined logical structure of repositories
and allow seamless integration with platform core services, like delivery pipelines (AKA CI/CD), IaC PR automation, etc.

Templates could be cloned and customized by the user and used for bootstrapping new workloads.

The default templates are

- [Workload code repository](https://github.com/CloudGeometry/cg-devx-wl-template)
- [Workload GitOps repository](https://github.com/CloudGeometry/cg-devx-wl-gitops-template)

Those templates provide you with:

- Workload repository structure
- Pre-defined Docker file
- CI process
- CD process
- Release promotion process
- GitOps style environment definition
- IaC for out of the cluster cloud resources

Additional templates will be provided later.

## Generic Workload template

### Code repository

Workload code repository is using the following structure:

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

The repository already contains pre-configured GitHub Actions (`.github` folder) and Argo Workflows (`.argo` folder)
to build new images and update workload manifests.

The service folder will have the name used when running boostrap command.

When using monorepo for multiple services,
each service should go under its own folder and has its own Dockerfile.

### GitOps repository

Workload GitOps repository is using the following structure:

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

The repository already contains pre-configured GitHub Actions (`.github` folder) and Argo Workflows (`.argo` folder)
required for GitOps style workload version promotion.