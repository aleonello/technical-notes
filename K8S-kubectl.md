# Kubectl commands

## Links

* [Kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [Get yaml from deployed kubernetes resources](https://jhooq.com/get-yaml-for-deployed-kubernetes-resources/)

## A way to set kubectl config

```sh
kubectl config set-cluster k8s --server="https://<server_address>"
kubectl config set clusters.k8s.certificate-authority-data <certificate_crt_code>
kubectl config set-credentials <user> --token="<jwt_token>"
kubectl config set-context default --cluster=k8s --user=<user>
kubectl config use-context default
```

## Generate resources file from a kubernetes cluster

1. Create directories for each namespace
```sh
kubectl get namespaces | awk 'NR>1{print $1}' | xargs -L 1 mkdir
```

2. Run the script below to access each directory and create the resource files
```sh
for i in $(find . -mindepth 1 -maxdepth 1 -type d -printf "%f\n"); do cd "${i}" &&  echo "processing: ${i}" && for n in $(kubectl get -n=${i} -o=name pv,pvc,configmap,serviceaccount,secret,ingress,service,deployment,statefulset,hpa,job,cronjob)
do
    mkdir -p $(dirname $n)
    kubectl get -o=yaml $n -n=${i/\.\//} > $n.yaml
done && cd ..; done
```

## Port Forward

```sh
kubectl port-forward <pod_name> <local_port>:<pod_port> -n <namespace>
```

## Copy a TLS secret to another service

```sh
kubectl get secret <existing_service_name> --namespace=<namespace> -oyaml > tls.yaml
```

Edit the tls.yaml file with the new namespace and service name values.

```sh
kubectl apply -f tls.yaml
```

## Connect to a pod terminal

* Pod

```sh
kubectl -n {NAMESPACE} exec --stdin --tty {POD} -- /bin/bash
```

* Container

```sh
kubectl -n {NAMESPACE} exec --stdin --tty {POD} --container {CONTAINER} -- /bin/sh
```

## Copy files from pod to bastion

```sh
kubectl cp <pod-name>:<fully-qualified-file-name> /<path-to-your-file>/<file-name> -c <container-name>
```

## Create Docker Registry Secret

```sh
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD
```

## List Ingress URLs

```sh
kubectl get --all-namespaces ingress -o json 2> /dev/null| jq -r '.items[] | .spec.rules[] | .host as $host | .http.paths[] | ( $host + .path)' | sort | grep -v ^/
```

## List all resources in a specific Namespace

```sh
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

```sh
kubectl get pod --all-namespaces | grep Evicted | awk '{ print $1" " $2 }' | xargs -n 2 kubectl delete pod --force --grace-period=0 -n
```

## Delete pods with more than one restart

```sh
kubectl get pods \
  --all-namespaces \
  --no-headers | awk '{ if ($5 > 1) { print $1 " " $2 } }' | xargs -n 2 \
kubectl delete pod --force --grace-period=0 -n
```

## Watch pods with more than 1 restart and show diffs

```sh
watch -d -n 10 "kubectl get pods -n dev | awk '{ if (\$4 > 1) { print \$0 } }'"
```

## Replicasets with more than 1 desired pod

```sh
kubectl get rs -n dev | awk '{ if ($2 != 1) { print $0 } }'
```

## How to know which Node the Pod is

```sh
kubectl get pods -o wide -n namespace | grep ms-name
```

## Deploys with missing pods

```sh
kubectl get deploy --all-namespaces | awk '{ if (NR == 1 || $4 != $5) { print $0 } }'
```

## Deploys with only one Replica

```sh
kubectl get deploy --all-namespaces | awk '{ if (NR == 1 || $4 == 1) { print $0 } }'
```

## Rollout Deployment

```sh
kubectl get deployment -A | sort | grep -vE "kube-system|istio|argocd|cert-manager" | sed 1d | awk '{ print "kubectl rollout restart deployment " $2 " -n " $1 }' | sh
```

## Rollout Deployments in background

```sh
ROLLOUT_DEPLOYMENT_SCRIPT="${HOME}/trash/rollout-deployments.sh"

echo '#!/bin/bash' > "${ROLLOUT_DEPLOYMENT_SCRIPT?}"

kubectl get deployment -A | grep -vE "kube-system|istio|argocd|cert-manager" | sed 1d | awk '{ print "kubectl rollout restart deployment " $2 " -n " $1 " &"}' | tee -a "${ROLLOUT_DEPLOYMENT_SCRIPT?}"

chmod +x "${ROLLOUT_DEPLOYMENT_SCRIPT?}"
```

## Pods with crashLoopBackOff

```sh
kubectl get pods -A | grep CrashLoopBackOff
```

## Scale UP/Down Deployment Replicas

```sh
kubectl scale deploy my-awesome-deployment --replicas=0 -n dev
```

## Scale UP/Down ALL Deployment Replicas

```sh
kubectl get deploy -n namespace | awk '{ if ($3 > 2) { print $1 } }' | xargs -n 1 kubectl scale deploy -n namespace --replicas=2
```

## Check pod memory usage

```sh
kubectl top pods --all-namespaces | awk 'NR==1 {print $0; next} {printf "%-30s%-30s%-15s%-15s\n", $1, $2, $3/1024 " Gi", $4}'
```
