# References by Technology

## Python

* [Comprehensive Python Cheatsheet](https://gto76.github.io/python-cheatsheet/)
* [The Hitchhiker’s Guide to Python](https://docs.python-guide.org/intro/learning/)
* [The Hitchhiker’s Guide to Python - Learning Path](https://docs.python-guide.org/intro/learning/)

## Helm Chart

### Search for a chart version

```
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm search repo secrets-store-csi-driver
```

## Kubernetes

### Node Selector and Toleration

```
  nodeSelector:
    name: devops
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "devops"
    effect: "NoSchedule"
```

## Sample Python application to run in K8S

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-apm-python-configmap
data:
  myscript.py: |
    from flask import Flask
    from elasticapm.contrib.flask import ElasticAPM


    app = Flask(__name__)


    @app.route("/")
    def hello():
        return "Hello, World!"


    if __name__ == "__main__":
        app.run(debug=True)




    # Configure to use ELASTIC_APM in your application's settings
    app.config['ELASTIC_APM'] = {
    # Set the required service name. Allowed characters:
    # a-z, A-Z, 0-9, -, _, and space
    'SERVICE_NAME': 'Python_Test',


    # Set the custom APM Server URL (default: http://localhost:8200)
    'SERVER_URL': 'http://apm-server-apm-server.elastic-stack.svc.cluster.local:8200',


    'SECRET_TOKEN': 'apmservertest',


    'VERIFY_SERVER_CERT': 'False',


    # Set the service environment
    'ENVIRONMENT': 'test',


    'DEBUG': 'True'
    }


    SERVER_URL = "http://apm-server-apm-server.apm-server.svc.cluster.local:8200"
    SERVICE_NAME = "Python_Test"
    ENVIRONMENT = "test"


    apm = ElasticAPM(app, server_url=SERVER_URL,service_name=SERVICE_NAME,environment=ENVIRONMENT)
  requirements.txt: |
    elastic-apm
    flask


---
apiVersion: v1
kind: Pod
metadata:
  name: test-apm-python
spec:
  containers:
  - name: execute-script
    image: python:3.9
    command:
    - "/bin/sh"
    - "-c"
    - >
      pip install -r /scripts/requirements.txt &&
      python /scripts/myscript.py
    volumeMounts:
    - name: script-volume
      mountPath: /scripts
  volumes:
  - name: script-volume
    configMap:
      name: test-apm-python-configmap
  restartPolicy: Never
```

### Kubectl

* [Kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

#### Copy a TLS secret to another service

```
kubectl get secret <existing_service_name> --namespace=<namespace> -oyaml > tls.yaml
```
Edit the tls.yaml file with the new namespace and service name values.

```
kubectl apply -f tls.yaml
```

#### Connect to a pod terminal

* Pod
```
kubectl -n {NAMESPACE} exec --stdin --tty {POD} -- /bin/bash
```
* Container
```
kubectl -n {NAMESPACE} exec --stdin --tty {POD} --container {CONTAINER} -- /bin/sh
```

#### Copy files from pod to bastion

```
kubectl cp <pod-name>:<fully-qualified-file-name> /<path-to-your-file>/<file-name> -c <container-name>
```
