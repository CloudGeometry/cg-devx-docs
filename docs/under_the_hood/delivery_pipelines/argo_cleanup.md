# Argo workflows cleanup options
To avoid cluster overload with completed pods Argo Workflow Controller has a cleanup feature. 
Cleanup parameters could be defined in a workflow and also in _workflow defaults_ subsection of helm _values_ in [application.yaml](https://github.com/CloudGeometry/cg-devx-core/blob/main/platform/gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/argo-workflows/application.yaml):
```yaml
          workflowDefaults:
            spec:
              podGC:
                strategy: OnWorkflowSuccess
              ttlStrategy:
                secondsAfterCompletion: 43200
                secondsAfterSuccess: 1800
                secondsAfterFailure: 21600
```
As shown above the declaration is self-explaining: 
`podGC strategy` is a pod garbage collector parameter, and `ttl` values define
the time to keep workflows after execution. After the complete cleanup
performed, the only option to observe the results of workflows is to look at
collected artifacts as well as archived logs which in their tur are also kind of
an artifact.

