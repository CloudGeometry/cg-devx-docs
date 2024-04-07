# Trivy
[Trivy](https://aquasecurity.github.io/trivy) is used to scan workload fs for vulnerabilities.  Trivy cluster workflowtemplate `trivy-fs-s3-cwft` takes the workload's repo checkout as an input hardwired artifact, scans the service directory and put report in appropriate location in S3 artifactory storage as an output artifacts in SARIF format for further observation. 
## Inputs:
- `{{workflow.parameters.repo}}`
- `{{workflow.parameters.tag}}`
- `{{workflow.parameters.dockerhub-registry-proxy}}`
- `{{workflow.parameters.workload-name}}`
- `{{workflow.parameters.wl-service-name}}`
- `{{workflow.parameters.wl-service-dir}}`
## Outputs:
```yaml
          - name: trivy-fs-report-sarif 
            path: /trivy-fs-report.sarif 
            s3:
              key: "{{workflow.parameters.workload-name}}/{{workflow.parameters.tag}}/{{workflow.parameters.wl-service-name}}-trivy-fs-report-sarif"
```
