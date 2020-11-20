### 1. Install Tekton Pipelines
    kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

### 2. Install Tekton Triggers
    kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml

### 3. deploy app1 and app2 on namespace ns1 and ns2
    kubectl apply -f deployments.yaml

### 3. Intall EventListener
    kubectl apply -f event-listener.yaml

### 3. Intall TriggerTemplate, Pipeline and task of the auto deployment pipeline
    kubectl apply -f pipeline-cd.yaml
    
### 4. Create the app1 triggerbinding's
    kubectl apply -f triggerbinding-app1.yaml
    
### 5. Use the same pipeline for app2
##### 5.1 Add this on the eventlistener manifest and apply the changes

```yaml
triggers:
  ...
  - name: app2-cd
     interceptors:
     - cel:
         filter: "body.repository.repo_name=='beopenit/app2'"
     bindings:
     - ref: app2-cd-binging
     template:
       ref: cd-template-pipeline
```
##### 5.2 Create the app2 triggerbinding's
    kubectl apply -f triggerbinding-app2.yaml
