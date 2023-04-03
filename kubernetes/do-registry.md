---
layout: default
title: Digital Ocean Registry
has_children: false
parent: Kubernetes
---

# Digital Ocean Registry


## Cleanup / Garbage collection

The cronjob:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: do-registry-gc
spec:
  schedule: "0 0 * * *" # This runs the job every day at midnight
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: do-registry-gc
            image: digitalocean/doctl:latest
            command: [ "/app/doctl" ]
            args: [ "registry", "garbage-collection", "start", "--force" ]
            env:
            - name: DIGITALOCEAN_ACCESS_TOKEN
              valueFrom:
                secretKeyRef:
                  name: do-api-token
                  key: token
          restartPolicy: Never
```

And the token:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: do-api-token
type: Opaque
data:
  token: ABCD..............
```
