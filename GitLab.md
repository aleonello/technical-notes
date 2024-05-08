# GitLab

## Create an empty pipeline

```yaml
stages:
- build

image: alpine

build:
stage: build
script:
- echo "Empty build"
```

## Create Role Binding and Cluster Role Binding

* gitlab-role.yaml

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
 name: gitlab-managed-apps
 namespace: agile-assessment-platform-prod
rules:
- apiGroups:
 - '*'
 resources:
 - '*'
 verbs:
 - '*'
```

* gitlab-role-binding.yaml

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
 name: gitlab-runner-cluster-admin
subjects:
- kind: ServiceAccount
 name: default
 namespace: gitlab-managed-apps
roleRef:
 kind: ClusterRole
 name: cluster-admin
 apiGroup: rbac.authorization.k8s.io
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
 name: runner-gitlab-runner
 namespace: agile-assessment-platform-prod
subjects:
- kind: ServiceAccount
 name: runner-gitlab-runner
 namespace: gitlab-managed-apps
roleRef:
 kind: Role
 name: gitlab-managed-apps
 apiGroup: rbac.authorization.k8s.io
```
