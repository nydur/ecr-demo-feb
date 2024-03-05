# 3.11 Continuous Deployment Container

Sample repository for assignment with practical walkthrough on Feb 20

---

## Main assignment objective

- Create ECR repository
- Build and push image to ECR
- Create ECS Cluster + Task Definition + Service

### Step 1

- Pre-requisites files of `app.py`, `requirements.txt` & `Dockerfile` already added to Github repository during coaching session on Feb 17

- Terraform infrastructure onfigurations to create ECR repository, ECS cluster, & Security group for ECS service within `main.tf`

- `terraform apply` to launch infrastructure

### Step 2

- Update image parameters using with `<ACCOUNT-ID>.dkr.ecr.<REGION>.amazonaws.com/<YOUR-ECR-NAME>:latest`

```tf
image     = "${data.aws_caller_identity.current.account_id}.dkr.ecr.${data.aws_region.current.name}.amazonaws.com/${local.prefix}-ecr:latest"
```

- Configurations set within `data.tf` & `main.tf` respectively will be retrieved to insert automatically

`ACCOUNT-ID` retrieved via `data.tf` for `${data.aws_caller_identity.current.account_id}`

`REGION` retrieved via `data.tf` for `${data.aws_region.current.name}`

`YOUR-ECR-NAME` retrieved via `main.tf` for `${local.prefix}`

- Update the security group inbound port to 8080, within `main.tf`

```tf
containerPort = 8080
```

- Re-run `terraform apply` to launch infrastructure

### Step 3

- Create workflow to deploy to ECS. Steps listed within `deploy-to-ecs.yml`

To run the workflow manually

```yaml
on:
  workflow_dispatch
```

To run the workflow on every `git push`

```yaml
on:
   push:
     branches: [ "main" ]
```
