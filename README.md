# tekton (navive Kubernetes CI/CD)

<br/>

**Create registry secret for accessing Azure Container Registry:**


```
$ kubectl create secret docker-registry docker-config \
    --docker-server="pruzickak8smyexampledev.azurecr.io" \
    --docker-username="${ARM_CLIENT_ID}" \
    --docker-password="${ARM_CLIENT_SECRET}"
```

<br/>

**Install Tekton with Dashboard:**

* tekton.yaml
* tekton-dashboard.yaml
* tekton-services.yaml

<br/>

**Prepare Tekton pipelines:**

* tekton-pipelineresource.yaml
* tekton-task-pipeline.yaml 

<br/>

**Initiate PipelineRun which builds container image form git repository:**

* tekton-pipelinerun.yaml


<br/>

**Check if the build of docker image was completed:**

    $ kubectl wait --timeout=30m --for=condition=Succeeded pipelineruns/podinfo-build-docker-image-from-git-pipelinerun

    $ kubectl get pipelineruns podinfo-build-docker-image-from-git-pipelinerun

<br/>

**Info:**   
https://ruzickap.github.io/k8s-flagger-istio-flux/part-03/


<br/>

**Taken from here:**  

https://github.com/ruzickap/k8s-flagger-istio-flux/tree/master/files/flux-repository
