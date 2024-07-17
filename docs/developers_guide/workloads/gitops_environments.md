# How to Model Your Environments

This is a reference implementation of a GitOps approach to environments management using Kustomize folders. The release
promotion model relies on this structure.

For more details, please see [this great example](
https://github.com/kostis-codefresh/gitops-environment-promotion)

## Understanding Workload configuration types:

These are the most common configuration types in the context of a Kubernetes application:

* **Application version** in the form of the container tag used.
  This is the most important setting from a promotion perspective.
  Often a change to the application version triggered by a source code change also requires a
  change to the runtime environment.

* **Kubernetes-specific settings** This includes the replica count, resource limits, health checks, persistent volumes,
  affinity rules, etc.

* (mostly) **Static business settings**.
  Application settings that are unrelated to Kubernetes, such as external URLs, authentication providers, connection
  strings, etc. These are "mostly static" — settings that are defined once for each environment and then never change.
  You don't want to promote these settings between environments.

* Non-static **business settings**.
  The same thing as static business settings, but you **do** want to promote them between environments.

## Folder structure

* The `base` folder holds configurations that are common to all environments.
  It is not expected to change often.
  If you want to make changes to multiple environments at the same time please use `variants`.

* The `variants` folder holds common characteristics between environments.
  It is up to you to define those commonalities.

* The `envs` folder holds individual environment configurations that are built from `base` and `variants`, and also
  includes
  environment-specific settings that are not common.

As for business settings, both static and non-static can come from envs, ConfigMap, Secret, and ExternalSecret, and are
Workload-specific; this reference implementation should be updated according to the needs of your application.

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

You can generate and compare effective manifests for each environment using the commands below:

```bash
# Current directory: GITROOT/gitops/environments

# make temp dir for file comparison
mkdir .tmp

# generate effective manifest
kustomize build envs/dev/> .tmp/dev.yaml
kustomize build envs/sta/ > .tmp/sta.yaml
kustomize build envs/prod/ > .tmp/prod.yaml

# diff envs manifests
vimdiff .tmp/dev.yaml .tmp/sta.yaml .tmp/prod.yaml

# drop temp dir
rm -rf .tmp/
```
