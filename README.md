# Megatron-Infra-Mgmt

Infrastructure management repository with automated directory structure creation.

## Features

### Automated Directory Structure Creation

This repository includes a GitHub Actions workflow that creates a standardized directory structure for infrastructure management.

#### Directory Structure

The workflow creates the following structure:

```
megatron-infra-mgmt/ 
├── tenants/
│   ├── {tenant-name}/
│   │     └── github/
│   │     └── {environment}/
│   │     │   └── cluster/
│   │     │   │   └── eks/
│   │     │   │   └── bootstrap/
│   │     │   └── networking/
└── shared-services/
```

#### Usage

1. Go to the **Actions** tab in the GitHub repository
2. Select the **Create Directory Structure** workflow
3. Click **Run workflow**
4. Fill in the input parameters:
   - **tenant_name**: Name of the tenant (e.g., `platform-team`)
   - **environments**: Comma-separated list of environments (e.g., `dev,staging,prod`)
   - **create_shared_services**: Whether to create the shared-services directory (default: `true`)
5. Click **Run workflow** to execute

The workflow will:
- Create the directory structure based on your inputs
- Add `.gitkeep` files to ensure empty directories are tracked by Git
- Commit and push the changes automatically

#### Example

With the default values:
- **tenant_name**: `platform-team`
- **environments**: `dev,staging,prod`
- **create_shared_services**: `true`

The workflow will create:
```
tenants/
└── platform-team/
    ├── github/
    ├── dev/
    │   ├── cluster/
    │   │   ├── eks/
    │   │   └── bootstrap/
    │   └── networking/
    ├── staging/
    │   ├── cluster/
    │   │   ├── eks/
    │   │   └── bootstrap/
    │   └── networking/
    └── prod/
        ├── cluster/
        │   ├── eks/
        │   └── bootstrap/
        └── networking/
shared-services/
```