# Terraform

## Links

* [Keep your CLI flags DRY](https://terragrunt.gruntwork.io/docs/features/keep-your-cli-flags-dry/)
* [Terraform CLI Documentation](https://developer.hashicorp.com/terraform/cli)
* [Terraform Language Documentation](https://developer.hashicorp.com/terraform/language)
* [Terraform AWS Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
* [Terraform Registry](https://registry.terraform.io/)
* [Terraform Best Practices](https://www.terraform-best-practices.com/)
* [Terraform State Management](https://developer.hashicorp.com/terraform/language/state)
* [Terraform Remote State](https://developer.hashicorp.com/terraform/language/state/remote)
* [Terragrunt Documentation](https://terragrunt.gruntwork.io/docs/)
* [Terragrunt GitHub Repository](https://github.com/gruntwork-io/terragrunt)

## Create plan JSON file

```sh
terragrunt plan -out=plan
terragrunt show -json plan > plan.json
```

## Terragrunt docker interactive command line

```sh
docker run --entrypoint /bin/sh --volume C:\terragrunt-dir\:/terragrunt --workdir /terragrunt -it alpine/terragrunt
```

Inside the container:

```sh
terragrunt hclfmt
```

## Terragrunt docker direct execution

```sh
docker run --volume C:\terragrunt-dir\:/terragrunt --workdir /terragrunt -t alpine/terragrunt hclfmt
```

## Terraform docker interactive command line

```sh
docker run --entrypoint /bin/sh --volume C:\terraform-dir\:/terraform --workdir /terraform -it hashicorp/terraform
```

Inside the container:

```sh
terraform fmt -recursive
```

## Terraform docker direct execution

```sh
docker run --volume C:\terraform-dir\:/terraform --workdir /terraform -t hashicorp/terraform terraform fmt -recursive
```
