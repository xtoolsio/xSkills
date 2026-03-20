---
name: azure-policy-verification
description: "Azure Policy verification using xPolicy CLI. This skill should be used when the user asks to 'verify a policy ID', 'check policy UUID', 'find Azure policies', 'search Azure policy', 'validate policy assignment', 'check policy definition', 'list policy categories', 'find policies for storage encryption', 'what policy has this UUID', 'is this policy deprecated', 'what policies are in this initiative', 'check policy references', 'verify policy description', 'compare policy definition', 'audit policy usage', 'validate initiative references', or any task involving Azure Policy definitions, policy assignments, policy sets, or initiatives in Terraform, OpenTofu, Bicep, ARM templates, or Pulumi. Also triggers on code containing policy_definition_id, policyDefinitionId, or /providers/Microsoft.Authorization/policyDefinitions/."
version: 0.1.0
author: Ahmad Obay (obay@xtools.com)
license: MIT
---

# Azure Policy Verification

Verify Azure Policy definitions, IDs, descriptions, and initiative references using the [xPolicy](https://xtools.com/xpolicy) CLI tool. xPolicy queries a local SQLite database of 4,400+ Azure Policy definitions and 230+ initiatives — no Azure API calls required.

## When to Apply

Apply this skill whenever code or documentation references Azure Policy and the agent needs to:

- Verify a policy UUID exists and is correct
- Confirm a policy description matches the actual definition
- Compare a local/custom policy JSON definition against the built-in catalog
- Find policies by keyword, category, or effect
- Validate initiative (policy set) references
- Check if referenced policies are deprecated, preview, or GA
- Bulk-verify policy references in Terraform, Bicep, ARM, or Pulumi code

## Prerequisites

xPolicy must be installed before use. **Always check first:**

```bash
command -v xpolicy >/dev/null 2>&1 && echo "installed" || echo "not installed"
```

If not installed:

```bash
# macOS / Linux
brew install xtoolsio/tap/xpolicy

# Windows
scoop bucket add xtoolsio https://github.com/xtoolsio/scoop-bucket
scoop install xpolicy
```

On first run, xpolicy automatically downloads the policy database to `~/.xpolicy/xPolicy.db`. No additional setup is needed.

## Core Commands

### Verify a Policy by UUID or Name

```bash
xpolicy policy get <uuid> --output json
xpolicy policy get "Display Name Here" --output json
```

If the UUID is invalid or not found, the command returns an error. Always use `--output json` for structured, parseable output.

### Search for Policies

```bash
# Full-text search across name and description
xpolicy policy list --search "encryption at rest" --output json --limit 10

# Filter by category
xpolicy policy list --category "Key Vault" --output json

# Filter by effect
xpolicy policy list --effect "Deny" --category "Storage" --output json

# Filter by cloud environment
xpolicy policy list --cloud "AzureUSGovernment" --output json --limit 10

# Combine filters
xpolicy policy list --type BuiltIn --state ga --search "network" --output json --limit 20
```

### Initiative (Policy Set) Lookup

```bash
# Get initiative by ID or name — returns all contained policies with details
xpolicy initiative get <uuid-or-name> --output json

# Search initiatives
xpolicy initiative list --search "NIST" --output json

# Filter by category
xpolicy initiative list --category "Regulatory Compliance" --output json

# Filter by type
xpolicy initiative list --type ALZ --output json
```

### Reference Data

```bash
xpolicy policy categories --output json       # All policy categories with counts
xpolicy policy effects --output json           # All policy effects with counts
xpolicy policy stats --output json             # Policy database statistics
xpolicy initiative categories --output json    # All initiative categories with counts
xpolicy initiative stats --output json         # Initiative database statistics
```

## Verification Workflows

### 1. Verify Policy UUID

When code references a policy ID like `06a78e20-9358-41c9-923c-fb736d382a4d`:

1. Run `xpolicy policy get <uuid> --output json`
2. Confirm it exists — report name, category, effect, and state (GA/preview/deprecated)
3. If not found, flag as invalid and suggest searching by keyword

### 2. Verify Policy Description

When code contains a policy UUID alongside a description or comment:

1. Run `xpolicy policy get <uuid> --output json`
2. Compare the `description` field against what appears in code
3. Report match or mismatch with the actual description

### 3. Verify Policy JSON Definition

When code contains a local/custom policy definition:

1. Extract the policy name or ID from the definition
2. Run `xpolicy policy get <id> --output json`
3. Compare `policyRule`, `parameters`, and `metadata` against the catalog version
4. Report any differences

### 4. Bulk Verification

When a Terraform file or Bicep template contains multiple policy references:

1. Extract all policy UUIDs from the code (look for `policy_definition_id`, `policyDefinitionId`, or resource ID paths containing `/providers/Microsoft.Authorization/policyDefinitions/`)
2. Verify each UUID with `xpolicy policy get <uuid> --output json`
3. Report which are valid, invalid, deprecated, or preview

## JSON Output Structure

Policy data returned by `xpolicy policy get --output json`:

```json
{
  "id": "06a78e20-9358-41c9-923c-fb736d382a4d",
  "name": "Audit VMs that do not use managed disks",
  "description": "This policy audits VMs that do not use managed disks",
  "version": "1.0.0",
  "category": "Compute",
  "mode": "All",
  "type": "BuiltIn",
  "preview": false,
  "deprecated": false,
  "effect": "Audit",
  "effectAllowedValues": "n/a",
  "rolesUsedCount": 0,
  "usedInPolicySetCount": 28,
  "usedInPolicySet": "CIS Controls v8.1 (...) BuiltIn; ...",
  "dateAdded": "",
  "cloudEnvs": "AzureCloud;AzureUSGovernment",
  "definition": "{ ... full JSON policy definition ... }"
}
```

Key fields to check: `deprecated` (boolean), `preview` (boolean), `type` (BuiltIn/Static/ALZ/AMBA/Community), `effect`, `category`, `usedInPolicySetCount`.

Initiative data returned by `xpolicy initiative get --output json` includes a `policies` array with each policy's `policyId`, `policyName`, `policyCategory`, `effectDefault`, `policyDeprecated`, and `policyPreview` fields.

## Important Notes

- **Always use `--output json`** for structured, parseable output
- **xpolicy is read-only** — queries a local database, no Azure API calls, cannot modify anything
- **Database auto-updates daily** — run `xpolicy update --force` only if the latest data is needed immediately
- **Extract UUIDs from resource IDs** — if code has `/providers/Microsoft.Authorization/policyDefinitions/06a78e20-...`, extract just the UUID portion
- **Filters are case-insensitive** — category, effect, and type filters work regardless of case
- **FTS5 search** — the `--search` flag uses SQLite full-text search; partial words and phrases work well
- **Policy types** include `BuiltIn`, `Static`, `ALZ`, `AMBA`, and `Community`

## Additional Resources

For detailed verification examples, IaC patterns, and complete command reference, consult:

- **`references/verification-workflows.md`** — Step-by-step verification examples for Terraform, Bicep, ARM, and Pulumi with expected outputs and error handling
- **`references/command-reference.md`** — Complete xpolicy CLI command reference with all flags, filters, and output formats
