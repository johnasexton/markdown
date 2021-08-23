# helm

### basic

```bash
brew install helm
```

### helm matching binary on local to binary on kubernetes/gke procedure

##### Overview:
* getting a specific version of helm binary to match a k8s cluster version of helm

```bash
$ which helm
/usr/local/bin/helm

$ /usr/local/bin/helm version
Client: &version.Version{SemVer:"v2.13.1", GitCommit:"618447cbf203d147601b4b9bd7f8c37a5d39fbb4", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.16.3", GitCommit:"1ee0254c86d4ed6887327dabed7aa7da29d7eb0d", GitTreeState:"clean"}

go to: https://github.com/helm/helm/releases
find desired version: https://github.com/helm/helm/releases/tag/v2.16.3
download correct version for your OS: https://get.helm.sh/helm-v2.16.3-darwin-amd64.tar.gz

$ cd ~/Downloads
$ mv helm-v2.16.3-darwin-amd64.tar.gz /usr/local/Cellar/helm/
$ cd /usr/local/Cellar/helm
$ tar -xzvf helm-v2.16.3-darwin-amd64.tar.gz
$ cd darwin-amd64/
$ ./helm version
$ rm -f /usr/local/bin/helm
$ ln -s /usr/local/Cellar/helm/darwin-amd64/helm /usr/local/bin/helm
which helm

$ helm version
Client: &version.Version{SemVer:"v2.16.3", GitCommit:"1ee0254c86d4ed6887327dabed7aa7da29d7eb0d", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.16.3", GitCommit:"1ee0254c86d4ed6887327dabed7aa7da29d7eb0d", GitTreeState:"clean"}
```

### Getting started with helm

# Helm @ CNCF / Kubecon 2020

##### An Introduction to Helm - Matt Farina, Samsung SDS & Josh Dolitsky, Blood Orange @ Cloud Native Computing Foundation / Kubecon 2020
* Talk: https://www.youtube.com/watch?v=Zzwq9FmZdsU
* Instructors: Matt Farina & Josh Dolitsky

### Helm's mantra
* helm was born to "install that other thing"

### INTRODUCTION TO HELM

### What is Helm?
* Package Manager for Kubernetes
* What is a package manager?
  * apt was inspiration for helm
  * those who know a system & how to run it take that knowledge & package it into a deliverable
  * this allows those without that specialized knowledge to still be able to benefit from it

### Comparison to apt
* with apt
```bash
sudo apt-get update
sudo apt-get install postgres postgres-contrib
```

* with Helm
```bash
helm repo add bitnami https://charts.bitnami.com
helm install mymaria bitnami/mariadb
```

### Is Helm Trustworthy?
* Helm is production ready according to security auditors
* 1M+ downloads per month
* Take semantic versioning very seriously, and will NOT break the API without declaring a MAJOR
* Power User's mailing list for Helm can engage with release candidates (RC)
* There are many maintainers, with cross sectional diversity, so it will not sink because of one company
* securely signed with PGP by a true maintainer stamp

### USING HELM
* Helm Hub: [hub.helm.sh](https://hub.helm.sh/)
* Helm Hub on GitHub: [github.com/helm/hub](https://github.com/helm/hub)
* [helm/monocular](https://github.com/helm/monocular) - another project hosted by helm, run this in your gke cluster & point it at your chart repository
* Helm Chart Museum - how to run your own Helm Chart Repository at [chartmuseum.com](https://chartmuseum.com/)
* helm/chart-testing - install & test charts that can use tests packaged in charts & can do linting
* helm/helm-2to3 plugin
* github.com/helm/helm-2to3
* helm/chart-releaser => tie it to CI
* many other strategic partnerships, ecosystems & tooling
* Helm3 Cloud Native experiments OCI repos to store helm charts => future road map feature
* most info on the internet are based on Helm version 2, so be careful reading docs

### Engaging with Helm
* Kubernetes Slack:
  * #helm-users
  * #helm-dev
  * #charts
* Mailing List - https://lists.cncf.io/g/cncf-helm
* Twitter - https://twitter.com/helmpack
* YouTube - https://www.youtube.com/helmpack
* Developer Call - Thursday at 9:30am PT

### Digging into Helm
* helm is made of packages, made of charts
* charts of made of several well known file types
* 'helm create app' creates a directory tree structure for a helm application called "app"
* Chart.yaml is your "helm chart"
* values.yaml is the default config
* templates/ <= contain manifest templates
  * service.yaml
  * deployment.yaml
  * hpa.yaml
  * ingress.yaml
  * etc...
* NOTES.txt
* _helpers.tpl

##### sample helm application called "template" example
* use helm binary to create a helm application directory structure skel named 'app'

```bash
$ helm create app
Creating app

$ tree app
app
├── Chart.yaml
├── charts
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

3 directories, 8 files
```
##### Charts.yaml
* describe your application
* also describe it's critical dependencies

##### values.yaml
* type of service: ClusterIP etc..

##### deployment.yaml
* {{ include "app.fullname" . }} indicate helm field / functions

Example:
```yaml
# Source: template/deplyment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "app.fullname" . }}
  labels:
    app.kubernetes.io/name: {{ include "app.name" . }}
    helm.sh/chart: {{ include "app.chart" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
    app.kubernetes.io/managed-by: {{ .Release.Service }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app.kubernetes.io/name: {{ include "app.name" . }}
      app.kubernetes.io/instance: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app.kubernetes.io/name: {{ include "app.name" . }}
        app.kubernetes.io/instance: {{ .Release.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /
              port: http
          readinessProbe:
            httpGet:
              path: /
              port: http
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
      {{- with .Values.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
      {{- end }}
    {{- with .Values.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
    {{- end }}
```

##### templates/_helpers.tpl
* special (optional) file for sections of yaml you may want to repeat all over & need a consistent set of labels as an example
* helm tests could be configured here
* in general templates/*.yaml are designed to avoid code duplication
* templates "look like" parameterized *.yaml but are truly Go template language

Example:

```go
$ cat templates/_helpers.tpl
{{/* vim: set filetype=mustache: */}}
{{/*
Expand the name of the chart.
*/}}
{{- define "app.name" -}}
{{- default .Chart.Name .Values.nameOverride | trunc 63 | trimSuffix "-" -}}
{{- end -}}

{{/*
Create a default fully qualified app name.
We truncate at 63 chars because some Kubernetes name fields are limited to this (by the DNS naming spec).
If release name contains chart name it will be used as a full name.
*/}}
{{- define "app.fullname" -}}
{{- if .Values.fullnameOverride -}}
{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" -}}
{{- else -}}
{{- $name := default .Chart.Name .Values.nameOverride -}}
{{- if contains $name .Release.Name -}}
{{- .Release.Name | trunc 63 | trimSuffix "-" -}}
{{- else -}}
{{- printf "%s-%s" .Release.Name $name | trunc 63 | trimSuffix "-" -}}
{{- end -}}
{{- end -}}
{{- end -}}

{{/*
Create chart name and version as used by the chart label.
*/}}
{{- define "app.chart" -}}
{{- printf "%s-%s" .Chart.Name .Chart.Version | replace "+" "_" | trunc 63 | trimSuffix "-" -}}
{{- end -}}

```
##### templates/NOTES.txt
* what it sounds like Notes

Example:

```go
$ cat templates/NOTES.txt
1. Get the application URL by running these commands:
{{- if .Values.ingress.enabled }}
{{- range $host := .Values.ingress.hosts }}
  {{- range .paths }}
  http{{ if $.Values.ingress.tls }}s{{ end }}://{{ $host.host }}{{ . }}
  {{- end }}
{{- end }}
{{- else if contains "NodePort" .Values.service.type }}
  export NODE_PORT=$(kubectl get --namespace {{ .Release.Namespace }} -o jsonpath="{.spec.ports[0].nodePort}" services {{ include "app.fullname" . }})
  export NODE_IP=$(kubectl get nodes --namespace {{ .Release.Namespace }} -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
{{- else if contains "LoadBalancer" .Values.service.type }}
     NOTE: It may take a few minutes for the LoadBalancer IP to be available.
           You can watch the status of by running 'kubectl get --namespace {{ .Release.Namespace }} svc -w {{ include "app.fullname" . }}'
  export SERVICE_IP=$(kubectl get svc --namespace {{ .Release.Namespace }} {{ include "app.fullname" . }} -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  echo http://$SERVICE_IP:{{ .Values.service.port }}
{{- else if contains "ClusterIP" .Values.service.type }}
  export POD_NAME=$(kubectl get pods --namespace {{ .Release.Namespace }} -l "app.kubernetes.io/name={{ include "app.name" . }},app.kubernetes.io/instance={{ .Release.Name }}" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl port-forward $POD_NAME 8080:80
{{- end }}
```
### Templates + Values = valid kubernetes yaml rendered, applied & configured in your cluster

##### helm install
* 'helm install myrelease ./myapp'
* this can be customized with custom values

Example:
```bash
# install with default values file
helm install myrelease ./myapp

# install with custom values file
helm install myrelease ./myapp -f custom.yaml

# merge multiple charts together in any combination (advanced)
helm install myrelease ./myapp \
  -f staging.yaml \
  -f us-east1.yaml \
  --set tracing.enabled=true
```
##### helm status
* helm status myrelease

Example:
```bash
helm status myrelease

<outputs details about release>
```

##### helm list
* helm package manager query

##### helm rollback

##### helm delete
* helm delete myrelease
