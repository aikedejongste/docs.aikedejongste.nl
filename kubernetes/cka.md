---
layout: default
title: CKA
has_children: false
parent: Kubernetes
---

# CKA

Check the pods in the kube-system namespace to see if the scheduler is running.

## Roles and role bindings

There are 4 different RBAC combinations and 3 valid ones:

Role + RoleBinding (available in single Namespace, applied in single Namespace)
ClusterRole + ClusterRoleBinding (available cluster-wide, applied cluster-wide)
ClusterRole + RoleBinding (available cluster-wide, applied in single Namespace)
Role + ClusterRoleBinding (NOT POSSIBLE: available in single Namespace, applied cluster-wide)


```bash
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development
kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
kubectl auth can-i update pods --as=john --namespace=development

k create clusterrole pipeline-deployment-manager --verb create,delete --resource deployments
```
## Clusterrolebinding that binds 2 service accounts to a clusterrole
```bash
k create clusterrolebinding pipeline-view --clusterrole view --serviceaccount ns1:pipeline --serviceaccount ns2:pipeline
```

## Verify
```bash
k auth can-i delete deployments --as system:serviceaccount:ns1:pipeline -n ns1

k auth can-i list deployments --as system:serviceaccount:ns1:pipeline -n ns1

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
