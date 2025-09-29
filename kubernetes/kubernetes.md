---
layout: default
title: Kubernetes
has_children: true
---

# Kubernetes

## Debug pod

```bash
kubectl run debug-tools --image=nicolaka/netshoot  --restart=Never -it -- /bin/bash
```

Possibly working yaml for this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: debug-tools
spec:
  securityContext:
    runAsNonRoot: true
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: debug-tools
    image: nicolaka/netshoot
    tty: true
    stdin: true
    securityContext:
      runAsNonRoot: true
      runAsUser: 1000             # pick a non-root UID
      allowPrivilegeEscalation: false
      capabilities:
        drop:
          - ALL
      seccompProfile:
        type: RuntimeDefault
    command: ["/bin/bash"]
```

## Install kubectl

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | tee /etc/apt/sources.list.d/kubernetes.list
```

## Install k3s SINGLE NODE

```yaml
curl -sfL https://get.k3s.io | sh -
k3s kubectl get node
```

## Install k3s CLUSTER (3 nodes)

```yaml
# on node1
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server --cluster-init
# on the other 2 nodes
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server --server https://<ip or hostname of server1>:6443

k3s kubectl get node
```

## Cluster certificate

```bash
kubectl config view --raw -o=jsonpath='{.clusters[0].cluster.certificate-authority-data}' | base64 --decode
```

## Copy file from local to pod

```bash
kubectl cp config.ini.php matomo/matomo-5b5c6c758d-9wdvm:/bitnami/matomo/config/config.ini.php
```

## K3s install with forced private ip

Source: [Github](https://github.com/alexellis/k3sup/issues/306#issuecomment-1059986048)

```bash
k3sup install --ip server_ip--user ubuntu --k3s-extra-args '--node-external-ip server_ip --node-ip server_ip'
k3sup join --user ubuntu --server-ip server_ip --ip agent_ip
```

## K3s examples for Hetzner Cloud

* [https://github.com/StarpTech/k-andy](https://github.com/StarpTech/k-andy)
* [https://github.com/cicdteam/terraform-hcloud-k3s](https://github.com/cicdteam/terraform-hcloud-k3s)

## Run something simple

```bash
kubectl run httpbin --image kennethreitz/httpbin --port 80 && kubectl expose pod httpbin --port 80
```

## Quick busybox shell

```bash
k run -it --rm --restart=Never busybox --image=gcr.io/google-containers/busybox sh
```

## Quick Ubuntu pod

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu
  labels:
    app: ubuntu
spec:
  containers:
  - name: ubuntu
    image: ubuntu:latest
    command: ["/bin/sleep", "1d"]
    imagePullPolicy: IfNotPresent
  restartPolicy: Always
EOF
```

## K9s (Kubernetes GUI)

Bash

```bash
wget https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_amd64.tar.gz -O /tmp/k9s.gz
cd /tmp && tar -zxvf /tmp/k9s.gz && mv /tmp/k9s /usr/local/bin/k9s && chmod +x /usr/local/bin/k9s
```

Ansible

```yaml
    - name: Download latest k9s binary
      get_url:
        url: "https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_amd64.tar.gz"
        dest: /tmp/k9s.tar.gz
      tags: [pre-reqs]

    - name: Extract k9s binary
      unarchive:
        src: /tmp/k9s.tar.gz
        dest: /tmp
        remote_src: true
      tags: [pre-reqs]

    - name: Install k9s binary
      copy:
        src: /tmp/k9s
        dest: /usr/bin/k9s
        remote_src: yes
        mode: 'a+x'
      tags: [pre-reqs]

    - name: Remove downloaded files
      file:
        path: /tmp/k9s*
        state: absent
      tags: [pre-reqs]
```

## Kubectl sort by time

```bash
kubectl get events -n <namespace> --sort-by=.metadata.creationTimestamp
```

## Patch a deployment with a new pull secret

```bash
kubectl patch deployment <deployment-name> -p '{"spec": {"template": {"spec": {"imagePullSecrets": [{"name": "my-pull-secret"}]}}}}'
```
