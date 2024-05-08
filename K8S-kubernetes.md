# Kubernetes

## Tools

* Windows Chocolatey script to install Kubernetes tools

```powershell
choco install kubernetes-cli -y
choco install kubernetes-helm -y
choco install k9s -y
choco install rancher-desktop -y
choco install terraform -y
choco install terraform-docs -y
choco install tflint -y
choco install terragrunt -y
choco install wsl2 -y
```

## Links

* [Ingress NGINX](https://github.com/kubernetes/ingress-nginx/tree/main/charts/ingress-nginx)
* [Storage Class](https://kubernetes.io/docs/concepts/storage/storage-classes/)
* [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
* [ArgoCD Persistent Volume](https://github.com/argoproj/argo-helm/issues/438)
* [External DNS addon](https://github.com/kubernetes-sigs/external-dns)
* [Injecting secrets into Kubernetes pods via Vault Agent containers](https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-sidecar)
* [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)
* [LongHorn - Distributed Block Storage - EBS](https://longhorn.io/)
* [OpenEBS - Distributed Block Storage - EBS](https://openebs.io/)
* [CloudQuery ELT](https://github.com/cloudquery/cloudquery)


## Node Selector and Toleration

```yaml
  nodeSelector:
    name: devops
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "devops"
    effect: "NoSchedule"
```

## Sample Python application to run in K8S

```yaml
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
