---
layout: default
title: CKA
has_children: false
parent: Kubernetes
---

# CKA

Check the pods in the kube-system namespace to see if the scheduler is running.

## Roles and role bindings

```bash
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development
kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
kubectl auth can-i update pods --as=john --namespace=development
```

## Temp pods

```bash
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc
kubectl run curl alpine/curl --rm -it -- sh
```

## Labels

```bash
kubectl get pods --show-labels
```

## Kubeconfig

```bash
kubectl get nodes --kubeconfig /root/CKA/admin.kubeconfig
```

## Exec in a pod:
```bash
kubectl exec -it non-root-pod -- id
```


  - image: busybox
    name: beta
    command: ["sleep", "4800"]
    env:
    - name: name
      value: beta


## Jsonpath

```bash
kubectl get nodes -ojson | jq -c 'paths' # Get the paths of the json

kubectl get pods -o=jsonpath='{.items[*].metadata.name}'
