# How to Model Your Environments

This is a reference implementation of a GitOps approach to the environments management using Kustomize folders. Release
promotion model is relying on this structure.

For more details, please see [this great example](
https://github.com/kostis-codefresh/gitops-environment-promotion)

## Understanding Workload configuration types:

Those are the most common configuration types in the context of a Kubernetes application:

* **Application version** in the form of the container tag used.
  This is the most important setting from promotion perspective.
  Often a change to application version triggered by source code change also requires a
  change to the runtime environment.

* **Kubernetes specific settings** This includes the replicas count, resource limits, health checks, persistent volumes,
  affinity rules, etc.

* (mostly) **Static business settings**.
  Settings of the application that are unrelated to Kubernetes, e.g. external URLs, authentication providers, connection
  strings, etc. So "mostly static" — settings that are defined once for each environment and then never change and you
  don't want to promote between environments.

* Non-static **business settings**.
  The same thing as the previous one, but you **do** want to promote them between environments.

## Folder structure

* `base` folder holds configuration which is common to all environments.
  It is not expected to change often.
  If you want to do changes to multiple environments at the same time please use `variants`.

* `variants` folder holds common characteristics between environments.
  It is up to you to define those commonalities.

* `envs` folder holds individual environments configuration that is built from `base` and `variants`, and also includes
  environment-specific settings that are not common.

As Business settings, both static and non-static could come from envs, ConfigMap, Secret, and ExternalSecret, and are
Workload-specific, this reference implementation should be updated accordingly.

```
.
├── base # <= common configuration
│   ├── deployment.yaml # <= kuernetes settings
│   ├── kustomization.yaml
│   ├── service.yaml # <= kuernetes settings
│   └── serviceaccount.yaml # <= kuernetes settings
├── envs # <= environment specific configuration
│   ├── dev
│   │   ├── cm.yaml # <= business settings
│   │   ├── deployment.yaml # <= kuernetes settings
│   │   ├── external-secrets.yaml # <= business settings
│   │   ├── ingress.yaml # <= kuernetes settings
│   │   ├── kustomization.yaml
│   │   ├── settings.yaml # <= business settings
│   │   └── version.yaml # <= application version 
│   ├── prod
│   │   ├── cm.yaml # <= business settings
│   │   ├── deployment.yaml # <= kuernetes settings
│   │   ├── external-secrets.yaml # <= business settings
│   │   ├── ingress.yaml # <= kuernetes settings
│   │   ├── kustomization.yaml
│   │   ├── settings.yaml # <= business settings
│   │   └── version.yaml # <= application version
│   └── sta
│       ├── cm.yaml # <= business settings
│       ├── deployment.yaml # <= kuernetes settings
│       ├── external-secrets.yaml # <= business settings
│       ├── ingress.yaml # <= kuernetes settings
│       ├── kustomization.yaml
│       ├── settings.yaml # <= business settings
│       └── version.yaml # <= application version
└── variants # <= common configuration between environments
    ├── dev
    │   ├── dev.yaml # <= kuernetes settings
    │   ├── kustomization.yaml
    │   ├── replicas.yaml # <= kuernetes settings replicas count
    │   └── settings.yaml # <= static settings
    ├── prod
    │   ├── kustomization.yaml
    │   ├── prod.yaml # <= kuernetes settings
    │   ├── replicas.yaml # <= kuernetes settings replicas count
    │   └── settings.yaml # <= static settings
    └── uat
        ├── kustomization.yaml
        ├── replicas.yaml # <= kuernetes settings replicas count
        ├── settings.yaml # <= static settings
        └── uat.yaml # <= other kuernetes settings

```

You could generate and compare effective manifest for each environment using commands below:

```bash
# Current directory: GITROOT/gitops/environments

# make temp dir for file comparison
mkdir .tmp

# generage effective manifest
kustomize build envs/dev/> .tmp/dev.yaml
kustomize build envs/sta/ > .tmp/sta.yaml
kustomize build envs/prod/ > .tmp/prod.yaml

# diff envs manifests
vimdiff .tmp/dev.yaml .tmp/sta.yaml .tmp/prod.yaml

# drop temp dir
rm -rf .tmp/
```