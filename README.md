# terraform-template

- Terraform template using TF modules and many environments
- Checks via: TF validate, TFlint, TFsec, Checkov via pre-commit and GHA

## Prerequisites (Tested on)

- tfenv 3.0.0

## CI/CD status

| Status |
| -------|
| ![Terraform-test GHA Workflow](https://github.com/andrey-asoskov/terraform-template/actions/workflows/terraform.yml/badge.svg) |

## Contents

- **.github/workflows/** - CI pipelines to test the Terraform config
- **Terraform/** - Terraform config
  - **components/** - Terraform components
  - **modules/** - Terraform modules
  - **.tflint.hcl** - TF Lint config
  - **tfsec.yml** - TF Sec config
- **.checkov.yaml** - Checkov config
- **.markdownlint.yaml**
- **.pre-commit-config.yaml** - Prec-commit config
- **.yamllint.yaml**

## Usage

### Test Terraform config

```commandline
cd Terraform/components/<component> #Select the component
terraform init
terraform validate

tflint -c ../../.tflint.hcl .

tfsec --config-file ../../tfsec.yml --concise-output .
tfsec --config-file ../../tfsec.yml --concise-output --tfvars-file ./terraform.auto.tfvars .

cd ../../..
checkov -d . --config-file ./.checkov.yaml
```

### Apply Terraform config

```commandline
cd Terraform/components/<component> #Select the component
terraform plan
terraform apply

terraform destroy
```

### Test GHA workflows locally

```commandline
# Test using medium image
act --rm -W ./.github/workflows/terraform.yml -P ubuntu-22.04=ghcr.io/catthehacker/ubuntu:act-22.04

# Test using full image 20.04
act --rm -W ./.github/workflows/terraform.yml -P ubuntu-22.04=catthehacker/ubuntu:full-20.04
```
