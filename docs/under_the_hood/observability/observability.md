# Under the hood: Observability

CG DevX reference implementation provides unified experience while working with observability stack.

## Monitoring

Default option for monitoring is based
on [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)

This chart also installs following charts:

- [prometheus-community/kube-state-metrics](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics)
- [prometheus-community/prometheus-node-exporter](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-node-exporter)
- [grafana/grafana](https://github.com/grafana/helm-charts/tree/main/charts/grafana)

For more details please
see [official documentation](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)

### Metrics collection

K8s state and K8s Node metrics are exported and collected by default.
All services that expose standard metrics endpoint (`/metrics`) and marked with labels `expose-metrics: "true"` are
discovered automatically and metrics are collected. To change this behavior or rename labels you should update
ServiceMonitor configuration for
workloads `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/monitoring/service-monitor-workloads.yaml`

### Dashboards

You could install additional dashboards or remove pre-installed ones by updating `dashboards` configuration section
of `kube-prometheus-stack` manifest in your platform GitOps
repository `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/monitoring/kube-prometheus-stack.yaml`

```yaml
dashboardProviders:
  dashboardproviders.yaml:
    apiVersion: 1
    providers:
      - name: 'awesome-grafana-dashboards-provider'
        orgId: 1
        folder: 'awesome-folder'
        type: file
        disableDeletion: true
        editable: true
        options:
          path: /var/lib/grafana/dashboards/awesome-grafana-dashboards-provider
dashboards:
  awesome-grafana-dashboards-provider:
    grafana-dashboards-name:
      url: 'path to dashboard json file'
      token: ''
 ```

### Alerts

To enable Slack alerts uncomment and update following configuration in your platform GitOps
repository `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/monitoring/kube-prometheus-stack.yaml`

```yaml
alertmanager:
  config:
    global:
      resolve_timeout: 10m
      slack_api_url: "https://hooks.slack.com/services/PLACE/HERE/YOURTOKEN"
    route:
      group_by: [ 'namespace' ]
      group_wait: 30s
      group_interval: 5m
      repeat_interval: 12h
      receiver: 'slack-notifications'
      routes:
        - receiver: 'null'
          matchers:
            - alertname =~ "InfoInhibitor|Watchdog"
        - receiver: 'slack-notifications'
          continue: true
    receivers:
      - name: 'null'
      - name: 'slack-notifications'
        slack_configs:
          - channel: 'alerts-channel'
            username: 'AlertBot'
            send_resolved: true
            title_link: ' '
            title: '[{{ .Status | toUpper }}{{ if eq .Status "firing" }}:{{ .Alerts.Firing | len }}{{ end }}] Monitoring Event Notification'
            text: >-
              {{ range .Alerts }}
                *Alert:* {{ .Annotations.summary }} - `{{ .Labels.severity }}`
                *Description:* {{ .Annotations.description }}
                *Details:*
                {{ range .Labels.SortedPairs }} • *{{ .Name }}:* `{{ .Value }}`
                {{ end }}
              {{ end }}
```

## Log management

Default log management implementation is based on Grafana [Loki](https://github.com/grafana/loki).
Loki is by design optimized to work with K8s pod logs. It allows you to seamlessly switch between metrics and logs using
the same labels greatly improving user experience. Loki is integrated with Grafana used for monitoring, and Grafana is
used as default user interface to query logs.

> Default configuration of Loki is based on PersistentVolumes and uses in-memory stores. It's not suitable for storing
> large amount of data with short retention period.

### Collection

Log collection is done automatically for all workloads.


### Log-based alerts
