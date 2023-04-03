---
layout: default
title: Envvars from values.yaml
parent: Helm
---

# Envvars from values.yaml


In deployment.tpl:
```yaml
{% raw %}
{{- range $key, $val := .Values.env }}
  - name: {{ $key }}
    value: {{ $val | quote }}
{{- end }}
{% endraw %}
```

and in values.yaml

```yaml
env:          
  FOO: "BAR"
  USERNAME: "CHANGEME"
  PASWORD: "CHANGEME"
```


OR

```yaml
envvars:                                                                        
  - name: DEFAULT_LOCALE                                                        
    value: nl            
```


and

```yaml
{% raw %}
env:                                                                  
{{- range .Values.envvars }}                                          
- name: {{ .name }}                                                   
  value: {{ .value }}                                                 
{{- end }}  
{% endraw %}
```
