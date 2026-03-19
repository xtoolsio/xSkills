---
name: azure-naming-convention
description: "Azure resource naming convention based on Microsoft Cloud Adoption Framework (CAF). This skill should be used when the user asks to 'create Terraform code for Azure', 'write OpenTofu for Azure', 'write Bicep for Azure', 'create ARM template', 'write Pulumi for Azure', 'name Azure resources', 'create Azure infrastructure', 'provision Azure resources', 'write azurerm resources', 'create a resource group', 'create a virtual machine', 'create a storage account', 'create a key vault', 'create an AKS cluster', 'create a virtual network', 'create a SQL database', 'create a Cosmos DB', 'review Azure naming', or any task involving Azure resource provisioning with Terraform, OpenTofu, Bicep, ARM templates, or Pulumi. Covers 150+ Azure resource types with standardized abbreviations and naming patterns."
version: 0.1.0
author: Ahmad Obay (obay@xtools.com)
license: MIT
---

# Azure Naming Convention

Enforce Microsoft Cloud Adoption Framework (CAF) naming conventions when provisioning Azure resources through any Infrastructure-as-Code tool (Terraform, OpenTofu, Bicep, ARM templates, Pulumi).

**Source:** https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming

## When to Apply

Apply this skill whenever IaC code creates, modifies, or references Azure resources. This includes:

- Writing new `azurerm_*` or `azapi_*` resource blocks (Terraform/OpenTofu)
- Authoring Bicep modules or ARM templates for Azure
- Writing Pulumi programs targeting Azure
- Reviewing existing IaC code for naming compliance

## Standard Naming Format

```
<abbreviation>-<workload>-<environment>-<region>-<instance>
```

Not all components are required for every resource type. Consult `references/abbreviations.md` for the specific format per resource.

### Naming Components

| Component | Description | Examples |
|-----------|-------------|---------|
| **Abbreviation** | CAF resource type prefix | `rg`, `vm`, `st`, `vnet`, `kv` |
| **Workload** | App, project, or workload name | `navigator`, `payments`, `shared` |
| **Environment** | Deployment stage | `prod`, `dev`, `qa`, `stage`, `test` |
| **Region** | Azure region short name | `westus`, `eastus2`, `westeu` |
| **Instance** | Zero-padded instance number | `001`, `002` |

### Core Rules

1. Place the abbreviation **first** in the name.
2. Use hyphens `-` as delimiters (except resources that prohibit them).
3. Use **lowercase** throughout.
4. Ensure names are unique within their scope (global, resource group, or parent resource).
5. Respect Azure length limits per resource type.

## No-Hyphen Resources

These resources require **alphanumeric-only** names — no hyphens, no special characters:

| Resource | Abbr | Format | Constraints | Example |
|----------|------|--------|-------------|---------|
| Storage account | `st` | `st<workload><###>` | 3-24 chars, lowercase | `stnavigatordata001` |
| Data Lake Store | `dls` | `dls<workload><env>` | Lowercase alphanumeric | `dlsnavigatorprod` |
| Container registry | `cr` | `cr<workload><env><###>` | 5-50 chars, alphanumeric | `crnavigatorprod001` |
| VM storage account | `stvm` | `stvm<workload><###>` | Lowercase alphanumeric | `stvmnavigator001` |
| AKS node pools | `npsystem`/`np` | `np<name>` | 1-12 chars, alphanumeric | `npsystem`, `npworker` |

## Quick Reference: Most Common Resources

| Resource | Abbr | Example |
|----------|------|---------|
| Resource group | `rg` | `rg-navigator-prod` |
| Virtual network | `vnet` | `vnet-prod-westus-001` |
| Subnet | `snet` | `snet-prod-westus-001` |
| Network security group | `nsg` | `nsg-weballow-001` |
| Public IP address | `pip` | `pip-navigator-prod-westus-001` |
| Load balancer (external) | `lbe` | `lbe-navigator-prod-001` |
| Load balancer (internal) | `lbi` | `lbi-navigator-prod-001` |
| Virtual machine | `vm` | `vm-sql-prod-001` |
| AKS cluster | `aks` | `aks-navigator-prod-001` |
| App Service plan | `asp` | `asp-navigator-prod` |
| Web app | `app` | `app-navigator-prod-001` |
| Function app | `func` | `func-navigator-prod-001` |
| Storage account | `st` | `stnavigatordata001` |
| Key vault | `kv` | `kv-navigator-prod` |
| SQL Database server | `sql` | `sql-navigator-prod` |
| SQL database | `sqldb` | `sqldb-users-prod` |
| Cosmos DB | `cosmos` | `cosmos-navigator-prod` |
| Container registry | `cr` | `crnavigatorprod001` |
| Log Analytics workspace | `log` | `log-navigator-prod` |
| Application Insights | `appi` | `appi-navigator-prod` |
| Managed identity | `id` | `id-navigator-prod-eastus2-001` |
| Azure OpenAI Service | `oai` | `oai-navigator-prod` |
| Private endpoint | `pep` | `pep-navigator-prod` |
| Firewall | `afw` | `afw-navigator-prod` |

For the **complete list of 150+ resource types** with resource provider namespaces and detailed formats, consult `references/abbreviations.md`.

## Scope Levels

Ensure name uniqueness within the correct scope:

| Scope | Description | Examples |
|-------|-------------|---------|
| **Global** | Unique across all of Azure (PaaS with public endpoints) | Storage accounts, web apps, Key Vaults, Cosmos DB, Container registries, SQL servers |
| **Resource group** | Unique within the resource group | VMs, VNets, NICs, NSGs, disks, load balancers |
| **Parent resource** | Unique within the parent resource | Subnets (within VNet), databases (within SQL server) |

## IaC Implementation

When writing Terraform or OpenTofu, apply the abbreviation as a prefix in every resource `name` argument. Use interpolation with standard variables:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "rg-${var.workload}-${var.environment}"
  location = var.location
}

resource "azurerm_virtual_network" "main" {
  name                = "vnet-${var.workload}-${var.location_short}-${var.instance}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  address_space       = ["10.0.0.0/16"]
}

# No hyphens for storage accounts
resource "azurerm_storage_account" "main" {
  name                     = "st${var.workload}${var.instance}"
  resource_group_name      = azurerm_resource_group.main.name
  location                 = azurerm_resource_group.main.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

For **comprehensive IaC examples** across all categories and all supported tools (Terraform, OpenTofu, Bicep, ARM, Pulumi), consult `references/naming-patterns.md`.

## Additional Resources

### Reference Files

For detailed abbreviations and naming patterns, consult:

- **`references/abbreviations.md`** — Complete list of 150+ Azure resource type abbreviations organized by category (AI/ML, Analytics/IoT, Compute, Containers, Databases, Developer Tools, DevOps, Integration, Management, Migration, Networking, Security, Storage, Virtual Desktop), with resource provider namespaces, naming formats, and examples
- **`references/naming-patterns.md`** — Detailed IaC code examples for every resource category in Terraform/OpenTofu, Bicep, ARM templates, and Pulumi, including recommended variable definitions and no-hyphen resource handling
