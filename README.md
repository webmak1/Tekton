# Example how to build and push docker container from local kubernetes cluster (minikube) to hub.docker.com registry by Tekton CI/CD


<br/>

From the begiinging...

Update tekton-pipeline-resource.yaml  
Replace webmakaka/podinfo:2.1.3 on your image.

<br/>

**Create registry secret for accessing Azure Container Registry:**


```
$ {
    export CONTAINER_REGISTRY_SERVER="https://index.docker.io/v1/"
    export CONTAINER_REGISTRY_USERNAME=your-docker-registry-username
    export CONTAINER_REGISTRY_PASSWORD=your-docker-registry-password
    export CONTAINER_REGISTRY_EMAIL=your-docker-registry-emai
}
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

**Official Doc:**  
https://github.com/tektoncd/pipeline/blob/master/docs/install.md


<br/>

    $ kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

    $ kubectl get pods --namespace tekton-pipelines --watch


<br/>

**Prepare Tekton pipelines:**

    $ kubectl apply -f ./tekton-pipeline-resource.yaml 
    $ kubectl apply -f ./tekton-task-pipeline.yaml 

<br/>

**Initiate PipelineRun which builds container image form git repository:**

    $ kubectl apply -f ./tekton-pipeline-run.yaml 

<br/>

    $ watch kubectl get pipelineruns podinfo-build-docker-image-from-git-pipelinerun

<br/>

    $ kubectl get tr
    $ tkn taskrun logs podinfo-build-docker-image-from-git-pipelinerun-build-doc-86dlt

<br/>

### Tekton Dashboard

**Official Doc:**  
https://github.com/tektoncd/dashboard


<br/>

    $ kubectl apply --filename https://github.com/tektoncd/dashboard/releases/download/v0.6.1/tekton-dashboard-release.yaml

    $ kubectl get pods --namespace tekton-pipelines


    $ kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097


localhost:9097

---

<strong>Marley</strong>

<a href="https://webmakaka.com"><strong>WebMakaka</strong></a>