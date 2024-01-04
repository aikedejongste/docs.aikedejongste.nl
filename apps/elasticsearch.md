---
layout: default
title: Elasticsearch
has_children: false
parent: Self Hosted Apps
---

# Elasticsearch

## Notes

Avoid having very large shards as this can negatively affect the cluster's ability to recover from failure. There is no fixed limit on how large shards can be, but a shard size of 50GB is often quoted as a limit that has been seen to work for a variety of use-cases.

A good rule-of-thumb is to ensure you keep the number of shards per node below 20 per GB heap it has configured. A node with a 30GB heap should therefore have a maximum of 600 shards, but the further below this limit you can keep it the better.


## Cleanup and data usage in Elastic

Execute these queries in the dev console `app/dev_tools#/console`

### Sort indexes by size

`GET _cat/indices?v&s=store.size:desc&h=index,store.size`

### Settings and ILM for index

`GET /.ds-logs-generic-default-2023.10.29-000004/_ilm/explain`

`GET /.ds-logs-generic-default-2023.10.29-000004/_settings`

### Oldest index

`GET _cat/indices?v&h=index,creation.date.string`

### See ILM policy for policy named test

`GET /_ilm/policy/test`


### Enable automatic deletion of data for logs policy

```
PUT _ilm/policy/logs
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "rollover": {
            "max_age": "30d",
            "max_primary_shard_size": "50gb"
          }
        }
      },
      "delete": {
        "min_age": "120d",
        "actions": {
          "delete": {}
        }
      }
    },
    "_meta": {
      "managed": true,
      "description": "default policy for the logs index template installed by x-pack"
    }
  }
}
```




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
