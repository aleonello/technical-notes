# Helm Chart for K8S

## Links

* [Helm Official Documentation](https://helm.sh/docs/)
* [Helm CLI Reference](https://helm.sh/docs/helm/)
* [Artifact Hub - Helm Chart Repository](https://artifacthub.io/)
* [Chart Best Practices Guide](https://helm.sh/docs/chart_best_practices/)
* [Chart Template Guide](https://helm.sh/docs/chart_template_guide/)
* [Values Files & Schema Validation](https://helm.sh/docs/topics/charts/#schema-files)
* [Helm GitHub Repository](https://github.com/helm/helm)

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

