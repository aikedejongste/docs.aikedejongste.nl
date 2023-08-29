---
layout: default
title: Elasticsearch
has_children: false
parent: Self Hosted Apps
---

# Elasticsearch

## 2 installation methods

* Helm Chart (<https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-install-helm.html>)
* Yaml Manifests (<https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-install-yaml-manifests.html>)

## Install Operator first

helm install elastic-operator elastic/eck-operator -n elastic-system --create-namespace

helm show values elastic/eck-operator

## Install a stack with Helm

It looks to me like this doesn't work with the basic (free) license. Or at least not for
fleet-server.

helm show values elastic/eck-stack

helm install stekker elastic/eck-stack -n elastic-stack --create-namespace -f stack-values.yaml

## Install a stack with Manifests

Only when using Traefik:

```bash
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.10/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
```

Enable logging so we can see what's happening:

```
apiVersion: helm.cattle.io/v1
kind: HelmChartConfig
metadata:
  name: traefik
  namespace: kube-system
spec:
  valuesContent: |-
    accessLogs:
      enabled: true
    debug:
      enabled: true
```

```
apiVersion: traefik.io/v1alpha1
kind: TLSStore
metadata:
  name: default
  namespace: kube-system
spec:
  defaultCertificate:
    secretName: default-cert
```

Apply your certificate in the secret 'default-cert'.

Add the dashboard route

### Start with the CRD's and the Operator

```bash
kubectl create -f https://download.elastic.co/downloads/eck/2.8.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.8.0/operator.yaml
```
