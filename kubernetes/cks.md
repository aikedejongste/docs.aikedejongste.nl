---
layout: default
title: CKS
has_children: false
parent: Kubernetes
---

# CKS

## Audit logs

Are configured in the kube-apiserver.yaml file.


## Create key and csr for user aike with common name

Set Common Name = aike@internal.users

```bash
openssl genrsa -out aike.key 2048 && openssl req -new -key aike.key -out aike.csr
```

## Manually sign the CSR with the k8s CA

This doesn't work because the CA is not a CA for users. It is a CA for the cluster.?????????????

```bash
openssl x509 -req -in aike.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out aike.crt -days 500
```

## Making Namespaces use Pod Security Standards works via labels.

`kubectl edit namespace team-red`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  labels:
    kubernetes.io/metadata.name: team-red
    pod-security.kubernetes.io/enforce: baseline # add
```

## Get secret from within a pod from the API:

```bash
curl https://kubernetes.default/api/v1/namespaces/restricted/secrets -H "Authorization: Bearer $(cat /run/secrets/kubernetes.io/serviceaccount/token)" -k
```

## Apply a APPArmor profile into the Linux kernel

```bash
root@cluster1-node1:~# apparmor_parser -q ./profile
```

Verify the profile is loaded:

```bash
root@cluster1-node1:~# aa-status
```

The deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: apparmor
  name: apparmor
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: apparmor
  template:
    metadata:
      labels:
        app: apparmor
    spec:
      nodeSelector:                          # add
        security: apparmor                   # add
      containers:
      - image: nginx:
        name: c3
        securityContext:                     # add
          appArmorProfile:                   # add
            type: Localhost                  # add
            localhostProfile: very-secure    # add
```

