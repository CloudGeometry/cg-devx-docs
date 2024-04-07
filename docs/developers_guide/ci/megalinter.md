# Megalinter
To lint all the source code [Megalinter by OX Security](https://megalinter.io/) is used. To avoid trying miriades of supported linters `.mega-linter.yml` is provided and 'flavor' image is used.
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
