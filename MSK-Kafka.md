# MSK Kafka

## Connect EKS pod to the MSK Kafka using IAM Auth

Create AWS IAM Role

`msk-kafka-test-role` file:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::<account_id>:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/<eks_cluster_id>"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "oidc.eks.us-east-1.amazonaws.com/id/<eks_cluster_id>:sub": [
                        "system:serviceaccount:kafka-test:kafka-test"
                    ]
                },
                "StringLike": {
                    "oidc.eks.us-east-1.amazonaws.com/id/<eks_cluster_id>:aud": "sts.amazonaws.com"
                }
            }
        }
    ]
}
```

* Create AWS IAM Policy

`msk-kafka-test-policy` file:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kafka-cluster:Connect",
                "kafka-cluster:AlterCluster",
                "kafka-cluster:DescribeCluster"
            ],
            "Resource": [
                "arn:aws:kafka:us-east-1:<account_id>:cluster/<kafka_cluster_name>/<kafka_cluster_id>"
            ]
        }
    ]
}
```

* Create service account with root priviledges (only for test purposes)

`kafka-test.yaml` file:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: kafka-test
  namespace: kafka-test
spec:
  containers:
  - name: kafka-test
    image: apache/kafka:3.7.0
    securityContext:
      allowPrivilegeEscalation: false
      runAsUser: 0
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: kafka-test
  namespace: kafka-test
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::<account_id>:role/msk-kafka-test-role
```

* Create a new namespace

```sh
kubectl create namespace kafka-test
```

* Apply the pod config to the EKS cluster

```sh
kubectl apply -f kafka-test.yaml -n kafka-test
```

* Access the pod terminal

```sh
kubectl -n kafka-test exec --stdin --tty kafka-test -- /bin/bash
```

* Install the busybox-tools to use Telnet

```sh
apk add busybox-extras
```

* Use Telnet to test the MSK Cluster connection

```sh
busybox-extras telnet <broker> <port>
```

* Create kafkaconfig file

`kafkaconfig` file
```conf
security.protocol=SASL_SSL
sasl.mechanism=AWS_MSK_IAM
sasl.jaas.config=software.amazon.msk.auth.iam.IAMLoginModule required;
sasl.client.callback.handler.class=software.amazon.msk.auth.iam.IAMClientCallbackHandler
```

* Get the aws-msk-iam-auth-<version>-all.jar file from https://github.com/aws/aws-msk-iam-auth/releases and put it to kafkaclient libs directory or export CLASSPATH=/aws-msk-iam-auth-<version>-all.jar

```sh
wget https://github.com/aws/aws-msk-iam-auth/releases/download/v2.1.0/aws-msk-iam-auth-2.1.0-all.jar
export CLASSPATH=./aws-msk-iam-auth-2.1.0-all.jar
```

* Execute the script `kafka-topics.sh` to perform the test

```sh
/opt/kafka/bin/./kafka-topics.sh --list --bootstrap-server <boker_endpoint>:9098 --command-config ./kafkaconfig
```

* Note: if there are authentication issues, try to export the AWS credentials using the commands below:

```sh
export AWS_ACCESS_KEY_ID=<aws_access_key_id>
export AWS_SECRET_ACCESS_KEY=<aws_secret_access_key>

```

* Delete pod from the EKS cluster

```sh
kubectl delete -f kafka-test.yaml
```

* Delete the Kafka namespace

```sh
kubectl delete namespace kafka-test
```

* References

* [How connect to MSK cluster from EKS cluster](https://stackoverflow.com/questions/69819087/how-connect-to-msk-cluster-from-eks-cluster)
* [Securing Apache Kafka is easy and familiar with IAM Access Control for Amazon MSK](https://aws.amazon.com/pt/blogs/big-data/securing-apache-kafka-is-easy-and-familiar-with-iam-access-control-for-amazon-msk/)
