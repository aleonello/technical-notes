# Kubectl commands

## Links
* [Kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [Get yaml from deployed kubernetes resources](https://jhooq.com/get-yaml-for-deployed-kubernetes-resources/)

## Copy a TLS secret to another service

```
kubectl get secret <existing_service_name> --namespace=<namespace> -oyaml > tls.yaml
```
Edit the tls.yaml file with the new namespace and service name values.

```
kubectl apply -f tls.yaml
```

## Connect to a pod terminal

* Pod
```
kubectl -n {NAMESPACE} exec --stdin --tty {POD} -- /bin/bash
```
* Container
```
kubectl -n {NAMESPACE} exec --stdin --tty {POD} --container {CONTAINER} -- /bin/sh
```

## Copy files from pod to bastion

```
kubectl cp <pod-name>:<fully-qualified-file-name> /<path-to-your-file>/<file-name> -c <container-name>
```

## Create Docker Registry Secret

```
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD
```

## List Ingress URLs

```
kubectl get --all-namespaces ingress -o json 2> /dev/null| jq -r '.items[] | .spec.rules[] | .host as $host | .http.paths[] | ( $host + .path)' | sort | grep -v ^/
```

## List all resources in a specific Namespace

```
kubectl api-resources \
  --verbs=list \
  --namespaced \
  --output name \
| xargs -n 1 \
  kubectl get \
    --show-kind \
    --ignore-not-found \
    --namespace "argocd" \
| tee namespace-resources.txt
```
## Delete pods in terminate State

```
kubectl get pod --all-namespaces | grep Evicted | awk '{ print $1" " $2 }' | xargs -n 2 kubectl delete pod --force --grace-period=0 -n
```

## Delete pods with more than one restart

```
kubectl get pods \
  --all-namespaces \
  --no-headers | awk '{ if ($5 > 1) { print $1 " " $2 } }' | xargs -n 2 \
kubectl delete pod --force --grace-period=0 -n
```

## Watch pods with more than 1 restart and show diffs

```
watch -d -n 10 "kubectl get pods -n dev | awk '{ if (\$4 > 1) { print \$0 } }'"
```

## Replicasets with more than 1 desired pod

```
kubectl get rs -n dev | awk '{ if ($2 != 1) { print $0 } }'
```

## How to know which Node the Pod is

```
kubectl get pods -o wide -n namespace | grep ms-name
```

## Deploys with missing pods

```
kubectl get deploy --all-namespaces | awk '{ if (NR == 1 || $4 != $5) { print $0 } }'
```

## Deploys with only one Replica

```
kubectl get deploy --all-namespaces | awk '{ if (NR == 1 || $4 == 1) { print $0 } }'
```

## Rollout Deployment

```
kubectl get deployment -A | sort | grep -vE "kube-system|istio|argocd|cert-manager" | sed 1d | awk '{ print "kubectl rollout restart deployment " $2 " -n " $1 }' | sh
```

## Rollout Deployments in background

```
ROLLOUT_DEPLOYMENT_SCRIPT="${HOME}/trash/rollout-deployments.sh"

echo '#!/bin/bash' > "${ROLLOUT_DEPLOYMENT_SCRIPT?}"

kubectl get deployment -A | grep -vE "kube-system|istio|argocd|cert-manager" | sed 1d | awk '{ print "kubectl rollout restart deployment " $2 " -n " $1 " &"}' | tee -a "${ROLLOUT_DEPLOYMENT_SCRIPT?}"

chmod +x "${ROLLOUT_DEPLOYMENT_SCRIPT?}"
```

## Pods with crashLoopBackOff

```
kubectl get pods -A | grep CrashLoopBackOff
```

## Scale UP/Down Deployment Replicas

```
kubectl scale deploy my-awesome-deployment --replicas=0 -n dev
```

## Scale UP/Down ALL Deployment Replicas

```
kubectl get deploy -n namespace | awk '{ if ($3 > 2) { print $1 } }' | xargs -n 1 kubectl scale deploy -n namespace --replicas=2
```
