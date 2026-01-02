# Megatron-Infra-Mgmt

Infrastructure management repository with automated tenant onboarding and Terraform configuration generation.

## Features

### Automated Tenant Onboarding

This repository includes a GitHub Actions workflow that creates a standardized directory structure for infrastructure management with automatic Terraform configuration generation.

#### Directory Structure

The workflow creates the following structure:

```
tenants/
└── {tenant-name}/
    ├── github/
    └── {location}/
        └── {environment}/
            ├── backend.tf
            └── init.tf
```

#### Usage

1. Go to the **Actions** tab in the GitHub repository
2. Select the **Onboard** workflow
3. Click **Run workflow**
4. Fill in the input parameters:
   - **tenant_name**: Name of the tenant (e.g., `platform-team`)
   - **environments**: Select an environment or `all` (options: `all`, `dev`, `staging`, `prod`)
   - **location**: AWS region/location (default: `eu-central-1`)
   - **provider_aws**: Include AWS provider (default: `true`)
   - **provider_postgresql**: Include PostgreSQL provider (default: `false`)
   - **provider_helm**: Include Helm provider (default: `false`)
   - **provider_kubernetes**: Include Kubernetes provider (default: `false`)
5. Click **Run workflow** to execute

The workflow will:
- Create the directory structure based on your inputs
- Generate `backend.tf` with S3 backend configuration
- Generate `init.tf` with selected Terraform providers and AWS provider configuration (if enabled)
- Commit and push the changes automatically

#### Generated Files

##### backend.tf
```hcl
terraform {
  backend "s3" {
    bucket         = "{env}"
    key            = "{tenant_name}/{location}/{env}|github.tfstate"
    region         = "{location}"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

##### init.tf
```hcl
terraform {
  required_version = ">= 1.8.0"

  required_providers {
    # Providers are conditionally included based on your selection
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
    # ... other selected providers
  }
}

# AWS provider is included if selected
provider "aws" {
  region = "{location}"
  default_tags {
    tags = {
      Environment = "{env}"
      Owner       = "{tenant_name}"
      Terraform   = "true"
    }
  }
}
```

#### Example

With inputs:
- **tenant_name**: `platform-team`
- **environments**: `all`
- **location**: `eu-central-1`
- **provider_aws**: `true`
- **provider_kubernetes**: `true`

The workflow will create:
```
tenants/
└── platform-team/
    ├── github/
    └── eu-central-1/
        ├── dev/
        │   ├── backend.tf
        │   └── init.tf
        ├── staging/
        │   ├── backend.tf
        │   └── init.tf
        └── prod/
            ├── backend.tf
            └── init.tf
```