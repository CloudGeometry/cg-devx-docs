# Workload Environments

A Workload may have multiple stages / environments, or instances of a workload with its associated configuration and resources.

Delivery pipelines automatically roll out changes to all stages, except live environment(s), which are usually
user-facing environments such as production, and by
default require manual promotion.

Each new workload has three (dev, sta, prod) environments with three (dev, uat, prod) different environment variants (
AKA kinds) associated with it, where one (prod) is the "live" environment.

Optionally, it’s possible to create a temporary environment (AKA an ephemeral environment) associated with a specific
feature branch with and environment
lifecycle tied to the branch lifecycle.
This can be used for feature testing in isolation or User Acceptance Testing (UAT).

An environment can be associated with a specific CG DevX Runtime cluster (K8s cluster).

![workload_environments](../../assets/diagrams.drawio)
