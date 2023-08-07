---
layout: default
title: YAML loop
parent: Helm
---

# YAML loop

Template for secrets

In values.yaml put:

```
namespaces:
  - test1
  - test2
```

Then in templates/secrets.tpl

```
{% raw %} 

{{- range .Values.namespaces }}
---
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
  namespace: {{ . | quote }}
type: Opaque
data:
  USER_NAME: asdfasdfasfdasfdasdf
  PASSWORD: asdfasdfasdfasdf
{{- end }}

{% endraw %}
```

Will give the following output:

```
---
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
  namespace: "test1"
type: Opaque
data:
  USER_NAME: asdfasfdasdfasd
  PASSWORD: asdfasfdasdfad
---
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
  namespace: "test2"
type: Opaque
data:
  USER_NAME: aasdfasdfasdf
  PASSWORD: asdfasdfasdfads
```
