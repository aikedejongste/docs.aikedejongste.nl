---
layout: default
title: Traefik on k3s
has_children: false
parent: Kubernetes
---

# Nginx ingress

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm install   cert-manager jetstack/cert-manager   --namespace cert-manager   --create-namespace   --version v1.3.0   --set installCRDs=true
helm install ingress-nginx ingress-nginx/ingress-nginx

k apply -f le-prod-issuer.yaml

k apply -f k3snode.company.nl.yaml

k --namespace default get services -o wide -w ingress-nginx-controller
```

## Ingress Controller

Install with: `helm install ingress-nginx ingress-nginx/ingress-nginx`

You can watch the status by running `kubectl --namespace default get services -o wide -w ingress-nginx-controller`

An example Ingress that makes use of the controller:

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: example
  namespace: foo
spec:
  rules:
    - host: www.example.com
      http:
        paths:
          - backend:
              serviceName: exampleService
              servicePort: 80
            path: /
  # This section is only required if TLS is to be enabled for the Ingress
  tls:
      - hosts:
          - www.example.com
        secretName: example-tls
```

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

```
apiVersion: v1
kind: Secret
metadata:
  name: example-tls
  namespace: foo
data:
  tls.crt: <base64 encoded cert>
  tls.key: <base64 encoded key>
type: kubernetes.io/tls
```
