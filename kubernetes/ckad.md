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

## Expose 1 pod

```bash
kubectl expose pod redis --port=1234 --target-port=6789 --name redis-service
```

Check if svc has an endpoint:

```bash
k -n aap describe svc project-aap-svc

or

k -n aap get ep project-aap-svc
```


## Expose a deployment or pods by label

```bash
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```

And then set the selector to the label of the deployment or pods

```yaml
spec:
  selector:
    your-label: redis
```

## Questions

1. Save a list of all namespaces to a file called `namespaces.txt`.
2. Create yaml for a pod with a single container running the `nginx` image with the tag `1.14.2` and name the container `nginx`.
3. Create a job with completions and paralellism set to 5, that runs the command `echo hello`.
4. Delete Helm chart
5. Upgrade a Helm chart


## Snippets

```yaml
command: ["/bin/sh"]
args: ["-c", "echo hello"]
```

```bash
helm get values .... -n namespace
helm list -a -A
```

```bash
k get secret my-secret -o jsonpath='{.data.password}'
```

```bash
k rollout status deployment/my-deployment
k rollout history deployment/my-deployment --revision=2
k rollout undo deployment/my-deployment --to-revision=2
```

```bash
valueFrom:
  secretKeyRef:
    name: my-secret
    key: password

mountPath: /etc/secrets
```

Env vars do not need a volumeMount

```bash
k -n moon create secret generic secret1 --from-literal user=test --from-literal pass=pwd
```

```yaml
- name: secret2-volume              # add
  secret:                           # add
    secretName: secret2             # add

  - name: secret2-volume            # add
    mountPath: /tmp/secret2         # add

  - name: SECRET1_USER              # add
    valueFrom:                      # add
      secretKeyRef:                 # add
        name: secret1               # add
        key: user                   # add

```


```bash
k -f /opt/course/14/secret-handler.yaml delete --force --grace-period=0
k -f /opt/course/14/secret-handler-new.yaml replace --force --grace-period=0
```

``bash
k -n moon create configmap name-ofi-it --from-file=index.html=/opt/course/14/index.html
```

```bash
k -n moon run tmp-name --restart=Never --rm --image=nginx:alpine -i -- curl -s name-ofi-it
```

```bash
k -n moon rollout restart deploy my-deployment
```

