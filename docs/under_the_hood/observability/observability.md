# Observability

The CG DevX reference implementation provides a unified experience while working with the observability stack.

## Monitoring

The default option for monitoring is based on
the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack).

This chart also installs the following charts:

- [prometheus-community/kube-state-metrics](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics)
- [prometheus-community/prometheus-node-exporter](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-node-exporter)
- [grafana/grafana](https://github.com/grafana/helm-charts/tree/main/charts/grafana)

For more details, please see
the [official documentation](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack).

### Metrics Collection

K8s state and K8s node metrics are exported and collected by default. All services that expose a standard metrics
endpoint (`/metrics`) and are marked with the label `expose-metrics: "true"` are discovered automatically and metrics
are collected. To change this behavior or rename labels, update the ServiceMonitor configuration for workloads
in `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/monitoring/service-monitor-workloads.yaml`.

### Dashboards

You can install additional dashboards or remove pre-installed ones by updating the `dashboards` configuration section of
the `kube-prometheus-stack` manifest in your platform GitOps
repository `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/monitoring/kube-prometheus-stack.yaml`.

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

To enable Slack alerts, uncomment and update the following configuration in your platform GitOps
repository `gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/monitoring/kube-prometheus-stack.yaml`.

Both snippets are included with the reference implementation. To enable or add new configurations, edit the promtail
manifest gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/promtail/promtail.yaml.
Log-Based Alerts

You can use log data provided by Loki to create log-based alerts in Grafana. For detailed instructions, please see the
official guide.

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


  ##Log Management

  The default log management implementation is based on Grafana Loki. Loki is optimized to work with K8s pod logs by design. It allows you to seamlessly switch between metrics and logs using the same labels, greatly improving the user experience. Loki is integrated with Grafana for monitoring, and Grafana is used as the default user interface to query logs.

  The default configuration of Loki is based on PersistentVolumes and uses in-memory stores. It's not suitable for storing a large amount of data with a short retention period.

  ##Collection

  Log collection is done automatically for all workloads using the promtail agent.

  Logs containing sensitive data can be filtered and data obfuscated using promtail's built-in functionality.

The snippet below enables IPv4 address and email obfuscation in log streams:

  ```yaml
pipelineStages:
  - cri: { }
  - match:
      # use sensitive data obfuscation only for a specific app
      selector: '{app="specific-app"}'
      stages:
        - replace:
            # IP4
            expression: '(\d{1,3}[.]\d{1,3}[.]\d{1,3}[.]\d{1,3})'
            replace: >-
              {{ printf "*IP4*{{ .Value | Hash \"salt\" }}*" }}
        - replace:
            # email
            expression: '([\w\.=-]+@[\w\.-]+\.[\w]{2,64})'
            replace: >-
              {{ printf "*email*{{ .Value | Hash \"salt\" }}*" }}

This snippet allows completely dropping logs by specific source label:

  ```yaml
extraRelabelConfigs:
  # drop logs for sources with the label drop-logs
  - source_labels: [ __meta_kubernetes_pod_label_drop_logs ]
    action: drop
    regex: true
```

Both snippets are included with the reference implementation. To enable or add new configurations, edit the promtail
manifest gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/promtail/promtail.yaml.

##Log-Based Alerts

You can use log data provided by Loki to create log-based alerts in Grafana. For detailed instructions, please see the
official guide.
