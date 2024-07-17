The CG DevX provides integration with Vulnerability and misconfiguration scaner as part of CI process. Aqua Security
Trivy is sed as default scaner. Result of the scan are stored
in [SARIF](https://docs.oasis-open.org/sarif/sarif/v2.1.0/sarif-v2.1.0.html) format and uploaded to Argo Workflow build
artifact storage and linked to Git. 
