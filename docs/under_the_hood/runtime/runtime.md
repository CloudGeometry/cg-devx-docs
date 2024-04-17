# Runtime

CG DevX extends K8s runtime by providing default monitoring, log management, and secrets management to all services.

## Isolation

CG DevX provides namespace based isolation for workloads. It is also possible to use cluster level isolation using
virtual clusters or physical clusters.
Workload namespaces are prefixed with `wl-` and following pattern `wl-<WL_NAME>-<ENV_NAME>`

![runtime](../../assets/diagrams.drawio)

## Autoscaling

[Cluster autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) is enabled by default with
CG DevX reference architecture. Cluster autoscaler configuration is cloud provider specific and may rely on additional
labels. Initial cluster node pool capacity is based on values specified in hosting provider terraform module. If you
change those values later, autoscaler configuration should be also
updates `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/kube-system/cluster-autoscaler/application.yaml`
