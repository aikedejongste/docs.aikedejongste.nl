---
layout: default
title: Traefik on k3s
has_children: false
parent: Kubernetes
---

# Traefik dashboard k3s

Dont forget to replace the hostname in the yaml below

```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: traefik-dashboard
  namespace: kube-system
spec:
  entryPoints:
    - web           # <-- using the web entrypoint, not the traefik (9000) one
  routes:           # v-- adding a host rule
    - match: Host(`traefik.aike.be`) && (PathPrefix(`/dashboard`) || PathPrefix(`/api`))
      kind: Rule
      services:
        - name: api@internal
          kind: TraefikService
```

## Configure Traefik on K3s

Use a HelmChartconfig

```yaml
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

## Traefik default certificate

### Install CRDs:

```bash
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.10/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
```

### Install TLSStore

```yaml
apiVersion: traefik.io/v1alpha1
kind: TLSStore
metadata:
  name: default
  namespace: kube-system
spec:
  defaultCertificate:
    secretName: default-certificate
```

### Create the secret with the cert

```bash
kubectl create secret generic default-certificate --from-file=tls.crt --from-file=tls.key -n traefik-system
```

### Use the certificate

```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: traefik-dashboard
  namespace: kube-system
spec:
  entryPoints:
    - websecure
  routes:
    - match: Host(`kibana.company.com`) && (PathPrefix(`/dashboard`) || PathPrefix(`/api`))
      kind: Rule
      services:
        - name: api@internal
          kind: TraefikService
  tls:
      secretName: default-certificate
```

or

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: elasticsearch-ingress
  namespace: elastic-system
  annotations:
    traefik.ingress.kubernetes.io/router.entrypoints: websecure
    traefik.ingress.kubernetes.io/router.tls: "true"
spec:
  tls:
    - hosts:
        - elastic.company.com
      secretName: default-certificate
  rules:
    - host: elastic.company.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: quickstart-es-http
                port:
                  number: 9200
```
