# Megalinter

To lint all the source code [Megalinter by OX Security](https://megalinter.io/) is used.
Linters configuration could be changed in supplied `.mega-linter.yml` using the configuration notes
at [https://megalinter.io/latest/config-file](https://megalinter.io/latest/config-file).
To avoid trying miriades of supported linters it's good idead to disable some of them in `.mega-linter.yml` or use '
flavor' image as [described](https://megalinter.io/latest/flavors/).
ENV variables and image could be changed
in [megalinter cluster workflow template](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/argo-workflows/cluster-workflow-templates/megalinter-cwft.yaml)
JSON report as well as SARIF are saved as S3 output artifacts for further observation.

## Inputs:

- `{{workflow.parameters.repo}}`
- `{{workflow.parameters.tag}}`
- `{{workflow.parameters.dockerhub-registry-proxy}}`
- `{{workflow.parameters.workload-name}}`
- `{{workflow.parameters.wl-service-name}}`
- `{{workflow.parameters.wl-service-dir}}`

## Outputs:

```yaml
- name: megalinter-report-sarif 
  path: /tmp/megalinter-report.sarif 
  s3:
    key: "{{workflow.parameters.workload-name}}/{{workflow.parameters.tag}}/{{workflow.parameters.wl-service-name}}-megalinter-report-sarif"
- name: megalinter-report-json 
  path: /tmp/mega-linter-report.json
  s3:
    key: "{{workflow.parameters.workload-name}}/{{workflow.parameters.tag}}/{{workflow.parameters.wl-service-name}}-megalinter-report-json"
```
