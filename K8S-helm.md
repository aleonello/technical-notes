# Helm Chart for K8S

## Search for a chart version

```sh
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm search repo secrets-store-csi-driver
```

## Helm list all services for all namespaces

```sh
helm ls --all-namespaces --all
```

## Helm Rollback example


```sh
helm rollback prometheus-stack 2 --namespace prometheus-stack
```

## Validate helm values

```sh
helm template ./ms-service | kubectl apply --dry-run -f -
```

## Helm Uninstall

```sh
helm uninstall <name> -n <namespace>
```

## Helm Template Example

```sh
helm template <name> <chart> -f <helm_value.yaml>
```
