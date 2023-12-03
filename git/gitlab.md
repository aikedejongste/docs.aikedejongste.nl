---
layout: default
title: GitLAB
parent: Git and Github
---

# GitLAB

## Gitlab runner with Helm

```bash
export RT=123456789
export NS=gitlabrunner

helm repo add gitlab https://charts.gitlab.io

helm install gitlab-runner -n $NS --set gitlabUrl=https://gitlab.com,runnerRegistrationToken=$RT gitlab/gitlab-runner
```

## Clone with deploy token

```bash
export GLUSER=username
export GLPASS=tokentokentoken

git clone https://$GLUSER:GLPASS@gitlab.com/org/project/repo.git
```



