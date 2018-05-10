# uw-it-helloworld
This repository is a demonstration continuous delivery pipeline.

k8s.yml
- Added RollingUpdate strategy to deployment spec
  i.e.
``` yml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
```
- Added the vsts BuildID to the Docker build and push tasks
``` shell
  imageName: 'lavielp/uw-it-helloworld:$(Build.BuildId)'
```
- Removed the :latest tag from the k8s deployment config file
- Created an k8s set image task in the build definition referencing the BuildId
``` yml
- task: Kubernetes@0
  inputs:
    kubernetesServiceConnection: 'ACS Kubernetes Down Level'
    command: set
    arguments: 'image deployment/helloworld helloworld=lavielp/uw-it-helloworld:$(Build.BuildId) --record'
```
