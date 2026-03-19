---
name: terraform-naming-convention
description: "Terraform file naming convention based on Obay's Terraform Naming Convention (OTN). This skill should be used when the user asks to 'organize Terraform files', 'rename Terraform files', 'name Terraform files', 'create Terraform file structure', 'split Terraform files', 'structure Terraform project', 'name a Terraform resource file', 'name a Terraform variable file', 'name a Terraform output file', 'name a Terraform module file', 'name a Terraform provider file', 'create a new Terraform file', 'add a Terraform resource', 'add a Terraform variable', 'add a Terraform output', 'add a Terraform module', 'add a Terraform data source', 'review Terraform file names', 'validate Terraform naming', 'clean up Terraform files', or any task involving creating, organizing, or naming Terraform and OpenTofu files. Enforces one block per file with standardized filename patterns across all block types."
version: 0.1.0
author: Ahmad Obay (obay@xtools.com)
license: MIT
---

# Terraform File Naming Convention

Enforce Obay's Terraform Naming Convention (OTN) when creating, organizing, or naming Terraform and OpenTofu files. The core principle is **one block per file**, named according to block type and identifier.

**Source:** https://www.obay.cloud/post/terraform-naming-convention/

## When to Apply

Apply this convention whenever creating, modifying, or organizing Terraform/OpenTofu files. This includes:

- Creating new resource, data, variable, output, module, or provider blocks
- Adding blocks to an existing Terraform/OpenTofu project
- Organizing or restructuring Terraform configurations
- Reviewing Terraform file structure for naming compliance
- Setting up new Terraform/OpenTofu projects

## Core Principle

**One block per file.** Place every Terraform block in its own file with a standardized name derived from the block type and identifiers. Never combine multiple blocks in a single file (except `locals.tf` and `terraform.tf`).

## Naming Patterns

| Block Type | Pattern | Example |
|------------|---------|---------|
| Resource | `resource.<provider_resource>.<name>.tf` | `resource.aws_instance.web.tf` |
| Data source | `data.<provider_resource>.<name>.tf` | `data.aws_ami.ubuntu.tf` |
| Variable | `variable.<name>.tf` | `variable.environment.tf` |
| Output | `output.<name>.tf` | `output.vpc_id.tf` |
| Module | `module.<name>.tf` | `module.vpc.tf` |
| Provider | `provider.<name>.tf` | `provider.aws.tf` |
| Provider (alias) | `provider.<name>.<alias>.tf` | `provider.aws.us-east-1.tf` |
| Locals | `locals.tf` | `locals.tf` |
| Terraform | `terraform.tf` | `terraform.tf` |

## Naming Rules

1. Use the **full resource type** including provider prefix for resources and data sources (e.g., `aws_instance`, `azurerm_virtual_network`).
2. Use the **block label** (the name assigned in HCL) as the last segment of the filename.
3. `locals.tf` and `terraform.tf` are **fixed filenames** — all local values go in `locals.tf`, and backend/provider requirements go in `terraform.tf`.
4. Provider files use the **provider name** only (e.g., `aws`, `azurerm`, `google`), with an optional alias segment when an alias is defined.
5. Use **lowercase** throughout all filenames.
6. Separate filename segments with **dots** (`.`).
7. Use **hyphens** (`-`) within descriptive block labels (e.g., `web-server`, `production-rg`).

## Before and After

### Before (Monolithic)

```
main.tf          (100+ lines, mixed resource/data/provider blocks)
variables.tf     (all variables in one file)
outputs.tf       (all outputs in one file)
```

### After (OTN-Compliant)

```
terraform.tf
locals.tf
provider.aws.tf
provider.aws.us-east-1.tf
variable.environment.tf
variable.region.tf
variable.instance_type.tf
data.aws_ami.ubuntu.tf
resource.aws_vpc.main.tf
resource.aws_subnet.public.tf
resource.aws_security_group.web.tf
resource.aws_instance.web.tf
module.monitoring.tf
output.vpc_id.tf
output.public_ip.tf
```

## Applying the Convention When Writing Code

When creating a new Terraform block, always place it in its own file following the naming pattern:

```hcl
# File: resource.azurerm_resource_group.main.tf
resource "azurerm_resource_group" "main" {
  name     = "rg-navigator-prod"
  location = "East US 2"
}
```

```hcl
# File: variable.environment.tf
variable "environment" {
  description = "Deployment environment"
  type        = string
  default     = "dev"
}
```

```hcl
# File: data.aws_ami.ubuntu.tf
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-*-amd64-server-*"]
  }
}
```

```hcl
# File: module.vpc.tf
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"
}
```

## Pre-Commit Workflow

Just as `terraform fmt` and `terraform validate` are standard steps before committing Terraform code, always run `tfren` to ensure all files follow OTN before committing changes to the repository:

```bash
terraform fmt -recursive     # Format code
terraform validate           # Validate configuration
tfren                        # Organize files into OTN-compliant structure
tfren validate --strict      # Verify naming compliance (fails on violations)
```

Integrate `tfren validate --strict` into CI/CD pipelines to enforce OTN compliance on every pull request.

## Automation with tfren

Use **tfren** (https://github.com/obay/tfren) to automatically organize existing Terraform projects into OTN-compliant structure.

### Install

```bash
brew install obay/tap/tfren                         # macOS/Linux
scoop bucket add obay https://github.com/obay/scoop-bucket.git && scoop install tfren  # Windows
```

### Common Commands

```bash
tfren                    # Organize: split + rename (default)
tfren -r                 # Organize recursively
tfren -n                 # Dry-run: preview changes
tfren split              # Split multi-block files only
tfren rename             # Rename files only
tfren validate           # Check naming compliance
tfren validate --json    # JSON output for CI/CD
tfren validate --strict  # Exit with error code on violations
```

### Flags

| Flag | Short | Description |
|------|-------|-------------|
| `--directory` | `-d` | Target directory (default: current) |
| `--recursive` | `-r` | Process subdirectories |
| `--dry-run` | `-n` | Preview changes without applying |
| `--force` | `-f` | Skip confirmation prompts |
| `--no-git-check` | | Skip git status check |
| `--quiet` | `-q` | Suppress non-error output |
| `--json` | | Output in JSON format |
| `--verbose` | `-v` | Verbose output |

## Additional Resources

### Reference Files

For detailed naming examples across all block types and cloud providers, consult:

- **`references/naming-patterns.md`** — Comprehensive examples for every block type (resource, data, variable, output, module, provider) across AWS, Azure, and GCP, with a complete project structure example and best practices
