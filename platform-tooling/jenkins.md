# jenkins-on-gke

### Documentation:
* https://www.jenkins.io/doc/book/installing/kubernetes/

### Jenkins Official Helm charts:
* https://github.com/jenkinsci/helm-charts

### Procedure:
* set context
* create namespace
* add helm repo
* apply manifests
* edit helm values
* apply helmchart & values deployment
* monitor & connect to resulting workloads

```bash
#!/usr/local/bin/bash
export GCPPROJECT=initialkubetest
export GKECLUSTER=gke_initialkubetest_us-east1-b_initialkubetest-e1-01
export REPOSPACE="jenkinsci"
export NAMESPACE="jenkins"
export CHART="${REPOSPACE}/${NAMESPACE}"
export CHARTSLOCALE="jenkinsci https://charts.jenkins.io"

# set context
gcloud config set project ${GCPPROJECT}
kubectx ${GKECLUSTER}

# create namespace
kubectl create namespace ${NAMESPACE}
kubectl get namespaces

# add helm repos
helm repo add ${CHARTSLOCALE}
helm repo update
helm search repo ${REPOSPACE}

# apply manifests & edit helm values
kubectl apply -f jenkins-volume.yaml
kubectl apply -f jenkins-sa.yaml
vim jenkins-values.yaml

# install jenkins via helm deployment
chart=${CHART}
helm install jenkins -n jenkins -f jenkins-values.yaml $chart
jsonpath="{.data.jenkins-admin-password}"
secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
echo $(echo $secret | base64 --decode)
jsonpath="{.spec.ports[0].nodePort}"
NODE_PORT=$(kubectl get -n jenkins -o jsonpath=$jsonpath services jenkins)
jsonpath="{.items[0].status.addresses[0].address}"
NODE_IP=$(kubectl get nodes -n jenkins -o jsonpath=$jsonpath)
echo http://$NODE_IP:$NODE_PORT/login
kubectl get pods -n jenkins
jsonpath="{.data.jenkins-admin-password}"
secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
echo $(echo $secret | base64 --decode)
jsonpath="{.data.jenkins-admin-password}"
kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath

# view status and connect to Jenkin's resources
kubectl get pods -n jenkins
kubectl get deployments -n jenkins
kubectl -n jenkins port-forward <pod_name> 8080:8080
```
