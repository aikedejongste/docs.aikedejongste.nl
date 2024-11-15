---
layout: default
title: User management
has_children: false
parent: Kubernetes
---

# User management

```yaml
---
- hosts: all
  vars:
    certs_path: "/home/aike/.certs"
    kubeconfig_root: "/home/aike/.kube/config.root"
    kubeconfig_user: "/home/aike/.kube/config"
    username: "dev-team"
    namespace: "test2000"
    k8s_address: "https://135.181.98.69:16443"

  tasks:
    - name: Check if there is a kubeconfig
      ansible.builtin.stat:
        path: "{{ kubeconfig_user }}"
      register: kubeconfig_stat

    - name: Create namespace
      kubernetes.core.k8s:
        api_version: v1
        kind: Namespace
        state: present
        kubeconfig: "{{ kubeconfig_root }}"
        verify_ssl: false
        name: "{{ namespace }}"

    - name: Generate private key
      ansible.builtin.shell: openssl genrsa -out "{{ certs_path}}/client.key" 2048
      when: not kubeconfig_stat.stat.exists

    - name: Generate certificate signing request (CSR)
      ansible.builtin.shell: openssl req -new -key "{{ certs_path }}/client.key" -out "{{ certs_path }}/client.csr" -subj "/CN=dev-team"
      when: not kubeconfig_stat.stat.exists

    - name: Sign the CSR with the Kubernetes CA
      ansible.builtin.shell: openssl x509 -req -in "{{ certs_path }}/client.csr" -CA "{{ certs_path }}/ca.crt" -CAkey "{{ certs_path }}/ca.key" -CAcreateserial -out "{{ certs_path }}/client.crt" -days '999'
      when: not kubeconfig_stat.stat.exists

#    - name: Read certificate files into variables
#      set_fact:
#        ca_crt_content_b64: "{{ lookup('file', certs_path + '/ca.crt') | b64encode }}"
#        client_crt_content_b64: "{{ lookup('file', certs_path + '/client.crt') | b64encode }}"
#        client_key_content_b64: "{{ lookup('file', certs_path + '/client.key') | b64encode }}"

    - name: Set up kubeconfig for new user
      copy:
        dest: "{{ kubeconfig_user }}"
        content: |
          apiVersion: v1
          kind: Config
          clusters:
          - cluster:
              certificate-authority-data: "{{ lookup('file', certs_path + '/ca.crt') | b64encode }}"
              server: "{{ k8s_address }}"
            name: kubernetes
          contexts:
          - context:
              cluster: kubernetes
              user: "{{ username }}"
              namespace: "{{ namespace }}"
            name: "{{ username }}-context"
          current-context: "{{ username }}-context"
          users:
          - name: "{{ username }}"
            user:
              client-certificate-data: "{{ lookup('file', certs_path + '/client.crt') | b64encode }}"
              client-key-data: "{{ lookup('file', certs_path + '/client.key') | b64encode }}"

    - name: Create ClusterRole
      kubernetes.core.k8s:
        kubeconfig: "{{ kubeconfig_root }}"
        state: present
        definition:
          apiVersion: rbac.authorization.k8s.io/v1
          kind: ClusterRole
          metadata:
            name: "{{ username }}-role"
          rules:
            - apiGroups: [""]
              resources: ["pods", "services", "deployments"]
              verbs: ["get", "list", "watch", "create", "update", "delete"]

    - name: Create RoleBinding
      kubernetes.core.k8s:
        kubeconfig: "{{ kubeconfig_root }}"
        state: present
        definition:
          apiVersion: rbac.authorization.k8s.io/v1
          kind: RoleBinding
          metadata:
            name: "{{ username }}-role-binding"
            namespace: "{{ namespace }}"
          subjects:
            - kind: User
              name: "{{ username }}"
              apiGroup: rbac.authorization.k8s.io
          roleRef:
            kind: ClusterRole
            name: "{{ username }}-role"
            apiGroup: rbac.authorization.k8s.io
```
