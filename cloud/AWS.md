# AWS

## Links

* [Udemy Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
* [IAM Json to Terraform](https://flosell.github.io/iam-policy-json-to-terraform/)
* [Amazon EKS identity-based policy examples](https://docs.aws.amazon.com/eks/latest/userguide/security_iam_id-based-policy-examples.html)
* [AWS Policy Simulator](https://policysim.aws.amazon.com/home/index.jsp?#groups)
* [AWS QuickSight Sign-In](https://docs.aws.amazon.com/quicksight/latest/user/signing-in.html)
* [Overriding EKS max pods using EKS module - Reddit](https://www.reddit.com/r/Terraform/comments/14ti9k1/overriding_eks_max_pods_using_eks_module/?rdt=41501)
* [Overriding EKS max pods using EKS module - GitHub](https://github.com/bottlerocket-os/bottlerocket/issues/3449)
* [EKS Networking - Prefix Mode for Linux](https://aws.github.io/aws-eks-best-practices/networking/prefix-mode/index_linux/)
* [Use AWS Secrets Manager secrets in Amazon EKS](https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating_csi_driver.html)
* [Create a VPC peering connection](https://docs.aws.amazon.com/vpc/latest/peering/create-vpc-peering-connection.html)
* [AWS and Kubecost collaborate to deliver cost monitoring for EKS customers](https://aws.amazon.com/pt/blogs/containers/aws-and-kubecost-collaborate-to-deliver-cost-monitoring-for-eks-customers/)
* [AWS Terraform Autoscaling Group With ALB Deployment Tutorial](https://devopscube.com/terraform-autoscaling-group/)
* [Kafka Security](https://medium.com/@stephane.maarek/introduction-to-apache-kafka-security-c8951d410adf)
* [Linux Bastion](https://aws.amazon.com/pt/solutions/implementations/linux-bastion/)
* [Secrets Manager vs Systems Manager Parameter Store](https://tutorialsdojo.com/aws-secrets-manager-vs-systems-manager-parameter-store/)
* [LocalStack Feature Coverage](https://docs.localstack.cloud/user-guide/aws/feature-coverage/)
* [Install K9s in AWS EC2 Instance](https://linux.how2shout.com/how-to-install-k9s-in-amazon-linux-2023/)
* [Max Pods per ENI - VPC CNI](https://github.com/aws/amazon-vpc-cni-k8s/blob/master/misc/eni-max-pods.txt)
* [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)
* [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
* [IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html)
* [Amazon EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
* [EKS Best Practices Guide](https://aws.github.io/aws-eks-best-practices/)
* [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
* [Amazon S3 CLI Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/index.html)
* [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
* [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
* [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
* [AWS Cost Explorer User Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)
* [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)

## Tools

Windows Chocolatey script to install AWS tools:

```sh
choco install awscli -y
```

## AWS CLI - Update EKS Kubeconfig

```sh
aws eks update-kubeconfig --region <region_code> --name <cluster_name>
```

## AWS CLI - Get EKS addons list

```sh
aws eks describe-addon-versions --query 'addons[*].addonName'
```

## AWS CLI - S3 List bucket content into separate files

```sh
for i in $(aws s3 ls | awk '{print $3}'); do echo "Bucket: $i" && aws s3 ls s3://$i --recursive > $i.txt; done
```

## AWS CLI - S3 Remove all objects from a bucket

```sh
aws s3 rm s3://bucket --recursive
```

## AWS CLI - S3 Delete bucket

```sh
aws s3 rb s3://bucket
```

## Install a Python package in a custom path for AWS Lambda import

```sh
pip3 install <package> -t <temp_folder>
```

## Enable Container Insights on an Amazon EKS cluster

Reference: [How do I turn on Container Insights metrics on an Amazon EKS cluster?](https://repost.aws/knowledge-center/cloudwatch-container-insights-eks-fargate)

`cwagent.yaml`:

```yaml
# deploy cwagent as daemonset
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: cloudwatch-agent
  namespace: amazon-cloudwatch
spec:
  selector:
    matchLabels:
      name: cloudwatch-agent
  template:
    metadata:
      labels:
        name: cloudwatch-agent
    spec:
      containers:
        - name: cloudwatch-agent
          image: public.ecr.aws/cloudwatch-agent/cloudwatch-agent:1.247360.0b252689
          #ports:
          #  - containerPort: 8125
          #    hostPort: 8125
          #    protocol: UDP
          resources:
            limits:
              cpu: 200m
              memory: 200Mi
            requests:
              cpu: 200m
              memory: 200Mi
          # Please don't change below envs
          env:
            - name: HOST_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.hostIP
            - name: HOST_NAME
              valueFrom:
                fieldRef:
                  fieldPath: spec.nodeName
            - name: K8S_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: CI_VERSION
              value: "k8s/1.3.16"
          # Please don't change the mountPath
          volumeMounts:
            - name: cwagentconfig
              mountPath: /etc/cwagentconfig
            - name: rootfs
              mountPath: /rootfs
              readOnly: true
            - name: dockersock
              mountPath: /var/run/docker.sock
              readOnly: true
            - name: varlibdocker
              mountPath: /var/lib/docker
              readOnly: true
            - name: containerdsock
              mountPath: /run/containerd/containerd.sock
              readOnly: true
            - name: sys
              mountPath: /sys
              readOnly: true
            - name: devdisk
              mountPath: /dev/disk
              readOnly: true
      nodeSelector:
        kubernetes.io/os: linux
      volumes:
        - name: cwagentconfig
          configMap:
            name: cwagentconfig
        - name: rootfs
          hostPath:
            path: /
        - name: dockersock
          hostPath:
            path: /var/run/docker.sock
        - name: varlibdocker
          hostPath:
            path: /var/lib/docker
        - name: containerdsock
          hostPath:
            path: /run/containerd/containerd.sock
        - name: sys
          hostPath:
            path: /sys
        - name: devdisk
          hostPath:
            path: /dev/disk/
      terminationGracePeriodSeconds: 60
      serviceAccountName: cloudwatch-agent
      tolerations:
        - key: node-role.kubernetes.io/master
          operator: Exists
          effect: NoSchedule
        - operator: "Exists"
          effect: "NoExecute"
        - operator: "Exists"
          effect: "NoSchedule"
```
