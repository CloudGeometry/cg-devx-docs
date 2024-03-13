# Workload Environments

Workload may have multiple stages / environments - instances of workload with associated configuration and resources.

Delivery pipelines will automatically roll out changes to all stages, except live kind of environment(s) (usually
user-facing environment AKA production) - which by
default require manual promotion.

Each new workload has three (dev, sta, prod) environments with three (dev, uat, prod) different environment variants (
AKA kinds) associated with it, where one (prod) is "live" environment.

Optionally, it’s possible to create a temporary environment (AKA ephemeral environment) associated with a specific
feature branch with environment
lifecycle tied to branch lifecycle.
This could be used for feature testing in isolation or User Acceptance Testing (UAT).

Environment could be associated with specific CG DevX Runtime cluster (K8s cluster).

![workload_environments](../../assets/diagrams.drawio)