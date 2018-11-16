# Node sites

## Overview

Node application are built into Docker images, which are stored on ECR.

The infrastructure is created using Terraform and is an ALB in front of an ASG that scales with CPU usage.
The servers in the ASG are a custom Ubuntu build that adds a few libraries like Docker, Docker Compose and AWS CLI.
Once the server spins up, it pulls secrets from AWS Parameter Store and runs a Docker image.

You have to know the basics of Terraform, but in general you can just do `terraform init` once and then `terraform apply` for every change in the infrastructure that you do.

## Saveing secrets

To save secrets into AWS Parameter Store, first check the secrets files `secrets_app_stg` (for staging) and `secrets_app_prd` (for production) and edit them as you see fit, then execute the following:

```bash
WWW_API_SECRETS_APP_STG=$(cat infra/terraform/secrets_app_stg)
aws ssm put-parameter --overwrite --name www_api_app_stg --value "$WWW_API_SECRETS_APP_STG" --type SecureString
WWW_API_SECRETS_APP_PRD=$(cat infra/terraform/secrets_app_prd)
aws ssm put-parameter --overwrite --name www_api_app_prd --value "$WWW_API_SECRETS_APP_PRD" --type SecureString
```
