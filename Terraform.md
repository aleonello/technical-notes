# Terraform

## Links

* [Keep your CLI flags DRY](https://terragrunt.gruntwork.io/docs/features/keep-your-cli-flags-dry/)

## Create plan json file

```
terragrunt plan -out=plan
terragrunt show -json plan > plan.json
```

## Terragrunt docker interactive command line

```
docker run --entrypoint /bin/sh --volume C:\terragrunt-dir\:/terragrunt --workdir /terragrunt -it alpine/terragrunt
```
* Inside container:
```
# terragrunt hclfmt
```

## Terragrunt docker direct execution

```
docker run --volume C:\terragrunt-dir\:/terragrunt --workdir /terragrunt -t alpine/terragrunt hclfmt
```

## Terraform docker interactive command line

```
docker run --entrypoint /bin/sh --volume C:\terraform-dir\:/terraform --workdir /terraform -it hashicorp/terraform
```
* Inside container:
```
# terraform fmt -recursive
```

## Terraform docker direct execution

```
docker run --volume C:\terraform-dir\:/terraform --workdir /terraform -t hashicorp/terraform terraform fmt -recursive
```
