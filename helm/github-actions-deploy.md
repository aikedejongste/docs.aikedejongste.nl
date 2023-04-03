---
layout: default
title: Github Actions deploy 
parent: Helm
---

# Github Actions Helm deploy without Chartmuseum

```yaml
{% raw %}

name: Helm Deploy

on:
  workflow_dispatch:
  workflow_call:

jobs:
  build:
    runs-on: self-hosted

    steps:
    - name: GitHub Environment Variables Action
      uses: FranzDiebold/github-env-vars-action@v2.7.0

    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Install Helm
      uses: azure/setup-helm@v3

    - name: Create kube dir for credentials
      run: mkdir -p $HOME/.kube

    - name: Save k8s credentials
      run: echo "${{ secrets.K8SCERT }}" | base64 -d > $HOME/.kube/config

    - name: Deploy new version
      run: helm upgrade -f helm-chart/values.prod.yaml company company/form-server -n name-space

    - name: Shred k8s credentials
      run: shred $HOME/.kube/config

{% endraw %}
```

# Github Actions Helm deploy WITH Chartmuseum

```yaml
{% raw %}
name: Helm Publish

on:
  workflow_dispatch:
  workflow_call:

jobs:
  build:
    # optionally run on self-hosted for fixed IP address
    #runs-on: self-hosted

    steps:
    - name: GitHub Environment Variables Action
      uses: FranzDiebold/github-env-vars-action@v2.7.0

    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Install Helm
      uses: azure/setup-helm@v3

    - name: Add Bitnami Helm repo
      run: helm repo add bitnami https://charts.bitnami.com/bitnami

    - name: Update Helm repos
      run: helm repo update

    - name: Install dependencies for Helm chart
      run: cd helm-chart && helm dependency update

    - name: Package Helm chart
      run: helm package helm-chart/

    #TODO: check exit code
    - name: Upload packaged Helm chart to Museum
      run: curl -s -u "github:${{ secrets.CHARTMUSEUMPASS }}" --data-binary "@`ls -1 *.tgz | head -n1`" https://charts.company.com/api/charts

    - name: Add Company Helm repo
      run: helm repo add company https://charts.company.com

    - name: Update Helm repos
      run: helm repo update

    - name: Create kube dir for k8s credentials
      run: mkdir -p $HOME/.kube

    - name: Save k8s credentials
      run: echo "${{ secrets.K8SCERT }}" | base64 -d > $HOME/.kube/config

    - name: Deploy new version
      run: helm upgrade -f helm-chart/values.yaml company company/app-name -n name-space

    - name: Shred k8s credentials
      run: shred $HOME/.kube/config

{% endraw %}
```
