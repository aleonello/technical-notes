# AWS

## Links
* [IAM Json to Terraform](https://flosell.github.io/iam-policy-json-to-terraform/)
* [Amazon EKS identity-based policy examples](https://docs.aws.amazon.com/eks/latest/userguide/security_iam_id-based-policy-examples.html)
* [AWS Policy Simulator](https://policysim.aws.amazon.com/home/index.jsp?#groups)
* [AWS QuickSight Sign-In](https://docs.aws.amazon.com/quicksight/latest/user/signing-in.html)

## Tools

* Windows Chocolatey script to install Development tools

```
choco install awscli -y
```

## Download Kubeconfig

```
aws eks update-kubeconfig --region region-code --name my-cluster
```

## Get EKS addons list

```
aws eks describe-addon-versions --query 'addons[*].addonName'
```
