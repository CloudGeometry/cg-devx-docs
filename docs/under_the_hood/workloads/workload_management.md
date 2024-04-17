# Workload Management

Workloads are managed by ArgoCD via the `ApplicationSet`
located at `/gitops-pipelines/delivery/clusters/cc-cluster/core-services/200-wl-argocd.yaml`
ArgoCD monitors the `/gitops-pipelines/delivery/clusters/cc-cluster/workloads` path for the CC cluster.
Per-workload definitions are based on a template
file, [workload-template.yaml](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/workloads/workload-template.yaml)
and added to that path by the CLI.
When manually adding workloads, you should either define your own workload `ApplicationSet` based on the template
and put it into the workloads folder, or create a new ArgoCD application.
