# tekton (navive Kubernetes CI/CD)

**Build and Push container to hub.docker.com registry.**

<br/>

From the begiinging...

Update tekton-pipeline-resource.yaml  
Replace webmakaka/podinfo:2.1.3 on your image.

<br/>

**Create registry secret for accessing Azure Container Registry:**


```
$ export CONTAINER_REGISTRY_SERVER="https://index.docker.io/v1/"
$ export CONTAINER_REGISTRY_USERNAME=your-docker-registry-username
$ export CONTAINER_REGISTRY_PASSWORD=your-docker-registry-password
$ export CONTAINER_REGISTRY_EMAIL=your-docker-registry-email
```

```
$ kubectl create secret docker-registry docker-config \
  --docker-server="${CONTAINER_REGISTRY_SERVER}" \
  --docker-username="${CONTAINER_REGISTRY_USERNAME}" \
  --docker-password="${CONTAINER_REGISTRY_PASSWORD}" \
  --docker-email="${CONTAINER_REGISTRY_EMAIL}"
```

<br/>

**Install Tekton with Dashboard:**

    $ kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

<br/>

* tekton-dashboard.yaml
* tekton-services.yaml

<br/>

**Prepare Tekton pipelines:**

    $ kubectl apply -f ./tekton-pipeline-resource.yaml 
    $ kubectl apply -f ./tekton-task-pipeline.yaml 

<br/>

**Initiate PipelineRun which builds container image form git repository:**

    $ kubectl apply -f ./tekton-pipeline-run.yaml 

<br/>

    $ kubectl get tr
    $ tkn taskrun logs podinfo-build-docker-image-from-git-pipelinerun-build-doc-86dlt

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
