---
layout: default
title: CKAD
has_children: false
parent: Kubernetes
---

# CKAD

## Coupon codes

- DCUBE20

## Courses

- [MatthewPalmer.net](https://matthewpalmer.net/kubernetes-app-developer/)

## Quick shell in pod

```bash
kubectl exec --stdin --tty shell-demo -- /bin/bash
```

## Show current kubeconfig

```bash
kubectl config --kubeconfig=config-demo view
```

## Add a cluster to kubeconfig

```bash
kubectl config --kubeconfig=config-demo set-cluster development --server=https://1.2.3.4 --certificate-authority=fake-ca-file
kubectl config --kubeconfig=config-demo set-cluster test --server=https://5.6.7.8 --insecure-skip-tls-verify
```

## Add users to kubeconfig

```bash
kubectl config --kubeconfig=config-demo set-credentials developer --client-certificate=fake-cert-file --client-key=fake-key-seefile
kubectl config --kubeconfig=config-demo set-credentials experimenter --username=exp --password=some-password
```

## Add contexts to kubeconfig

```bash
kubectl config --kubeconfig=config-demo set-context dev-frontend --cluster=development --namespace=frontend --user=developer
kubectl config --kubeconfig=config-demo set-context dev-storage --cluster=development --namespace=storage --user=developer
kubectl config --kubeconfig=config-demo set-context exp-test --cluster=test --namespace=default --user=experimenter
```

## Forward a port from localhost to a pod

```bash
kubectl port-forward mongo-75f59d57f4-4nd6q 28015:27017
or
kubectl port-forward deployment/mongo 28015:27017
```





