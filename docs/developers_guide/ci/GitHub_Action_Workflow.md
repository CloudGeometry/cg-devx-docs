# Github Action Workflow Structure
CI Github Action start is triggered by push new tag to the workload repo. 
All the CI chain elements run from the steps of job `multi_service_parallel_build` described in `multi_service_parallel_build.yml`. If one of the steps fails, the Github action fails completely. Some steps are simple bash executed on the runner, another are submissions of relatively complex argo workflows. The workflow structure is linear:  steps are executed one after another. 
```mermaid
---
title: multi_service_parallel_build  workflow chart
---
flowchart 
n1["`**semver_chk**`"
cheks if tag matchs SEMVER format
]--if not SEMVER tag, exit-->
n2["`**actions/checkout@v4**`"
]-->
n3["`**argo_cli_install**
installs _argo cli_  on runner`"]-->
n4["`**build**`"
checks build conditions, detects repo changes and submits workflow of services build workflows 
]-- if any build fails, exit -->
n41["`**trivy_libs**
    if _#quot;apps#quot;_ structure, submits _trivy-libs-wf_ workflow to scan shared libs source code in _libs_ directory. 
`"]-->
n5["`**registry_put**
submits _crane_ workflow to tag and put images to Harbor`"
]-- if any put fails, exit -->
n6["`**up_tags**
submits _crane-img-tag_ workflow to up the tag for unchanged services (if any)
`"]-->
n7["`**version_change**
submits _version_change_ workflow to change version in gitops repo
`"]
```
