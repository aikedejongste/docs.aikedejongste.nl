---
layout: default
title: KCNA
has_children: false
parent: Kubernetes
---

# KCNA

Transport Security > Authentication > Authorization > Admission Control

What is the successor to PodSecurityPolicies? Admission Controller

A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources.

Values:
- Distribution is better than centralization
- Community over product or company
- Automation over process
- Inclusive is better than exclusive
- Evolution is better than stagnation

Layers (OPA PRO):
* Orchestration & Management
* Provisioning
* App definition and development
* Platform
* Runtime
* Observability & analysis

The Container Runtime Interface (CRI) is the main protocol for the communication between the kubelet and Container Runtime. The Kubernetes Container Runtime Interface (CRI) defines the main gRPC protocol for the communication between the node components kubelet and container runt

Kubernetes supports multiple authorization modules, such as ABAC mode, RBAC Mode, and Webhook mode

Workload Rightsizing

- VerticalPodAutoscaler — adjusting CPU, Mem of Pods
- HorizontalPodAutoscaler — add or remove pods to meet the demand
- Cluster Downsizing Opportunities
- Auto scaler Add or remove Nodes to meet the demand

Use a load balancer to do a canary deploy.
Canary is a deployment method to mitigate risk not to evaluate features.

Field selectors let you select Kubernetes objects based on the value of one or more resource fields

If you want to control traffic flow at the IP address or port level (OSI layer 3 or 4), then you might consider using Kubernetes NetworkPolicies for particular applications in your cluster.

**Istio uses Envoy proxy**

**Linkerd, Consul and Istio are service meshes.**

Kubernetes has the following opinions about cluster networking:
- all Pods can communicate with all other Pods without using NAT
- all Nodes can communicate with all Pods without using NAT.
- the IP that a Pod sees itself as, is the same IP that others see it as

Microservices:
- Multiple apps are each responsible for one thing
- Functionality is isolated and stateless
- Highly maintainable and testable
- Independently deployable
- Organized around business capabilities

A service mesh is an infrastructure layer that can provide the following:
- Reliability (Traffic Management, Retries, Load Balancing)
- Observability (Metrics, Traces)
- Security (TLS Certifications, Identity)

k api-resources and k api-versions both exist. api-groups does not.

dark deployments deal with features in the front-end rather than the backend as is the case with canaries.

RuntimeClass is a feature for selecting the container runtime configuration

cgroups are for ISOLATION

What is the name for a service that has no clusterIp address? Headless

*Knative is serverless, but it's not a framework.*

Fission is a serverless framework.

Service Mesh = control plane + data plane

In Kubernetes there are 3 types of selectors:

1. Label Selector - Select K8s objects based on the applied label

2. Field Selector - Select K8s objects based on object data eg. Metadata, Status

3. Node Selector - Select nodes for very specific pod placement

Label Selectors define labels as a key value pair under metadata in a Manifest file

6443 + 8080

* kubectl config view  (KCV)
* kubectl EDIT resource (KER)

There are two areas of concern for securing Kubernetes: - Securing the cluster components that are configurable - Securing the applications which run in the cluster

A workload is an application running on Kubernetes.

You can use either labels or annotations to attach metadata to Kubernetes objects

etcd is the primary data source in k8s

apiVersion determines which api group will be used

Labels are key/value pairs that are attached to objects


