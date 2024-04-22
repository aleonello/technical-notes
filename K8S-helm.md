# Helm Chart for K8S

## Search for a chart version

```
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm search repo secrets-store-csi-driver
```

## Helm list all services for all namespaces

```
helm ls --all-namespaces --all
```

## Helm Rollback example


```
helm rollback prometheus-stack 2 --namespace prometheus-stack
```

## Validate helm values

```
helm template ./ms-service | kubectl apply --dry-run -f -
```