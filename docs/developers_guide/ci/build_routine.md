# Build routine execution
```mermaid
---
title: "Build routine invocation"
---
flowchart 
n1["`Github Action Step 
__build__`"]
subgraph "./argo/build-wow-wf.yaml"
    n2(build-loop-xxxx)
end
subgraph "`build-service-SN-xxxx`"
    n3(["`build-chain-p-cwft:
DAG:
    - kaniko-s3-p-cwft
    - megalinter-cwft
    - trivy-fs-s3-cwft
`"])
end
subgraph "`build-service-SN-yyyy`"
    n4(["`build-chain-p-cwft:
DAG:
    - kaniko-s3-p-cwft
    - megalinter-cwft
    - trivy-fs-s3-cwft

`"])
end
subgraph "`build-service-SN-zzzz`"
    n5(["`build-chain-p-cwft:
DAG:
    - kaniko-s3-p-cwft
    - megalinter-cwft
    - trivy-fs-s3-cwft
`"])
end
n1-->n2-->n3 & n4 & n5
```
