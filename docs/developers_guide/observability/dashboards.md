# List of Dashboards

[//]: # (TODO: Replace external generic links with own)

## K8s

A modern set of [Grafana dashboards for Kubernetes](https://github.com/dotdc/grafana-dashboards-kubernetes).

Dashboard for Prometheus
![Dashboard for Prometheus](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-addons-prometheus.png)

Dashboard for the API Server Kubernetes component
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-api-server.png)

Show information on the CoreDNS Kubernetes component
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-coredns.png)

`Global` level view dashboard for Kubernetes
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png)

`Namespaces` level view dashboard for Kubernetes
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-namespaces.png)

`Nodes` level view dashboard for Kubernetes
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-nodes.png)

`Pods` level view dashboard for Kubernetes
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-pods.png)

### USE

The Utilization Saturation and Errors (USE) Method dashboard that can be used to quickly identify resource bottlenecks
or errors.

## Security

### Trivy

Dashboard for the Aqua Security Trivy Operator
![](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-addons-trivy-operator.png)

### Kyverno

![1](https://grafana.com/api/dashboards/15987/images/11905/image)
![2](https://grafana.com/api/dashboards/15987/images/11906/image)
![3](https://grafana.com/api/dashboards/15987/images/11907/image)

## Cost

### KubeCost cluster metrics

Dashboard integrated with KubeCost providing view on findings and insights.
This dashboard gives your Kubernetes cluster costs:

- Cluster Wide (Live and Estimative)
    - Relative price of spot instances
- Namespace (Live and Estimative)
    - Price variation between days and weeks
- APP (Live and average)
    - Price comparison with 7 days ago
- PVC Costs

![1](https://grafana.com/api/dashboards/11270/images/8942/image)
![2](https://grafana.com/api/dashboards/11270/images/8943/image)
![3](https://grafana.com/api/dashboards/11270/images/8944/image)
![4](https://grafana.com/api/dashboards/11270/images/8945/image)

## Node.js

### Node.js metrics

Based on [this template](https://grafana.com/grafana/dashboards/11956-nodejs-metrics/)

This dashboard works with the metrics exported by the prom-client package for node.js.
CG DevX Log management is pre-configured to automatically collect metrics exported by node.js application.

![1](https://grafana.com/api/dashboards/11956/images/7764/image)
![2](https://grafana.com/api/dashboards/11956/images/7765/image)

