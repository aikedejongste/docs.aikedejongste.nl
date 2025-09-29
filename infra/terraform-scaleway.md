---
layout: default
title: Terraform Scaleway
parent: Cloud Infrastructure
has_children: false
---

# Terraform Scaleway

## Error page for load balancer

```hcl
resource "scaleway_object_bucket" "errorpage" {
  name   = "errorpage"
  region = "nl-ams"
}

resource "scaleway_object_bucket_acl" "errorpage_acl" {
  bucket = scaleway_object_bucket.errorpage.id
  acl    = "public-read"
}

resource "scaleway_object_bucket_website_configuration" "errorpage" {
    bucket = scaleway_object_bucket.errorpage.id
    index_document {
      suffix = "index.html"
    }
    error_document {
      key = "error.html"
    }
}

resource "scaleway_object" "errorpage_index" {
  bucket = scaleway_object_bucket.errorpage.id
  key    = "index.html"
  content = file("${path.module}/index.html")
}

resource "scaleway_object" "errorpage_error" {
  bucket = scaleway_object_bucket.errorpage.id
  key    = "error.html"
  content = file("${path.module}/error.html")
```
