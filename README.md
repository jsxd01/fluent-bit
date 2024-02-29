# Elastic Cloud on Kubernetes (ECK) Deployment Guide

This guide provides step-by-step instructions for deploying ECK, a tool for managing Elasticsearch clusters on Kubernetes, along with Kibana and Fluent Bit for logging and visualization.

## 1. Deploying ECK

### 1.1. Install Custom Resource Definitions:

$ kubectl create -f https://download.elastic.co/downloads/eck/2.11.1/crds.yaml

### 1.2. Install the Operator with its RBAC Rules:

$ kubectl apply -f https://download.elastic.co/downloads/eck/2.11.1/operator.yaml

### 1.3. Deploy ECK:

$ kubectl apply -f elasticsearch.yaml

### 1.4. Retrieve Elasticsearch Password:

$ PASSWORD=$(kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
$ echo $PASSWORD

## 2. Deploying Kibana Instance:

$ kubectl apply -f kibana.yaml

*(Optional: Forward Port)*
$ kubectl port-forward service/quickstart-kb-http 5601

## 3. Deploying Fluent Bit:

Before deployment, replace the ES password placeholder in fluentbit-values.yaml (line 393): HTTP_Passwd <es-password>.

$ helm repo add fluent https://fluent.github.io/helm-charts

$ helm install -f values.yaml --generate-name fluent/fluent-bit

## 4. Accessing Logs in Kibana:

To view logs in Kibana, forward the port:
$ kubectl port-forward service/quickstart-kb-http 5601

Then, navigate to localhost:5601 in your browser.
