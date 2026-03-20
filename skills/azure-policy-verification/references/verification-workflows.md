# Verification Workflows

Detailed step-by-step workflows for verifying Azure Policy references in Infrastructure-as-Code.

## Terraform / OpenTofu

### Single Policy Assignment

Given this Terraform code:

```hcl
resource "azurerm_resource_group_policy_assignment" "audit_vms" {
  name                 = "audit-vms-dr"
  resource_group_id    = azurerm_resource_group.main.id
  policy_definition_id = "/providers/Microsoft.Authorization/policyDefinitions/0015ea4d-51ff-4ce3-8d8c-f3f8f0179a56"
  description          = "Audit virtual machines without disaster recovery configured"
}
```

**Verification steps:**

1. Extract the UUID: `0015ea4d-51ff-4ce3-8d8c-f3f8f0179a56`
2. Run:
   ```bash
   xpolicy policy get 0015ea4d-51ff-4ce3-8d8c-f3f8f0179a56 --output json
   ```
3. Check the response:
   - Does the policy exist? If error, the UUID is invalid.
   - Does the `name` (display name) match the intent of the assignment?
   - Is `deprecated` false?
   - Does the `description` match what's in the Terraform code?
   - Is the `effect` appropriate for the use case?

### Multiple Policy Assignments (Bulk)

Given a Terraform file with multiple assignments:

```hcl
locals {
  policy_assignments = {
    "audit-vms-dr" = {
      policy_id   = "0015ea4d-51ff-4ce3-8d8c-f3f8f0179a56"
      description = "Audit VMs without DR"
    }
    "deny-public-ip" = {
      policy_id   = "83a86a26-fd1f-447c-b59d-e51f44264114"
      description = "Deny public IP addresses"
    }
    "require-tag" = {
      policy_id   = "96670d01-0a4d-4649-9c89-2d3abc0a5025"
      description = "Require a tag on resources"
    }
  }
}
```

**Verification steps:**

1. Extract all UUIDs from the map
2. Verify each one:
   ```bash
   xpolicy policy get 0015ea4d-51ff-4ce3-8d8c-f3f8f0179a56 --output json
   xpolicy policy get 83a86a26-fd1f-447c-b59d-e51f44264114 --output json
   xpolicy policy get 96670d01-0a4d-4649-9c89-2d3abc0a5025 --output json
   ```
3. For each policy, report:
   - Valid or invalid UUID
   - Display name (so the user can confirm intent)
   - Deprecated/preview status
   - Whether the description in code matches the actual description

### Initiative Assignment

```hcl
resource "azurerm_resource_group_policy_assignment" "nist" {
  name                 = "nist-sp-800-53"
  resource_group_id    = azurerm_resource_group.main.id
  policy_definition_id = "/providers/Microsoft.Authorization/policySetDefinitions/179d1daa-458f-4e47-8086-2a68d0d6c38f"
}
```

**Verification steps:**

1. Detect this is an initiative (contains `policySetDefinitions`)
2. Extract UUID: `179d1daa-458f-4e47-8086-2a68d0d6c38f`
3. Run:
   ```bash
   xpolicy initiative get 179d1daa-458f-4e47-8086-2a68d0d6c38f --output json
   ```
4. Report the initiative name, category, number of contained policies, and deprecated/preview status

## Bicep

### Policy Assignment in Bicep

```bicep
resource policyAssignment 'Microsoft.Authorization/policyAssignments@2022-06-01' = {
  name: 'audit-storage-encryption'
  properties: {
    displayName: 'Audit storage accounts without encryption'
    policyDefinitionId: '/providers/Microsoft.Authorization/policyDefinitions/b2982f36-99f2-4db5-8efa-04529a570b2b'
    description: 'Storage accounts should use customer-managed key for encryption'
  }
}
```

**Verification steps:**

1. Extract UUID from `policyDefinitionId`: `b2982f36-99f2-4db5-8efa-04529a570b2b`
2. Run:
   ```bash
   xpolicy policy get b2982f36-99f2-4db5-8efa-04529a570b2b --output json
   ```
3. Compare:
   - Does `displayName` in Bicep match the policy's actual `name`?
   - Does `description` match?
   - Is the policy GA (not deprecated or preview)?

## ARM Templates

### Policy Assignment in ARM

```json
{
  "type": "Microsoft.Authorization/policyAssignments",
  "apiVersion": "2022-06-01",
  "name": "deny-public-endpoints",
  "properties": {
    "displayName": "Deny public network access",
    "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1c6e92c9-99f0-4e55-9cf2-0c234dc48f99",
    "description": "Deny creation of resources with public endpoints"
  }
}
```

**Verification steps:**

Same as Bicep — extract UUID, run `xpolicy policy get`, compare fields.

## Pulumi

### Policy Assignment in Pulumi (Python)

```python
assignment = azure_native.authorization.PolicyAssignment(
    "audit-sql-encryption",
    policy_assignment_name="audit-sql-encryption",
    policy_definition_id="/providers/Microsoft.Authorization/policyDefinitions/a8bef009-a5c9-4d0f-90d7-6b04b5e0cbbc",
    display_name="Audit SQL DB encryption",
    scope=resource_group.id,
)
```

**Verification steps:**

1. Extract UUID: `a8bef009-a5c9-4d0f-90d7-6b04b5e0cbbc`
2. Run `xpolicy policy get a8bef009-a5c9-4d0f-90d7-6b04b5e0cbbc --output json`
3. Compare `display_name` against the actual policy name

## Finding the Right Policy

When writing new policy assignments and you need to find the correct policy:

### By Keyword

```bash
# Find policies related to storage encryption
xpolicy policy list --search "storage encryption" --output json --limit 10

# Find deny policies for networking
xpolicy policy list --search "network" --effect "Deny" --output json --limit 10
```

### By Category

```bash
# List all Key Vault policies
xpolicy policy list --category "Key Vault" --output json

# List all Compute deny policies
xpolicy policy list --category "Compute" --effect "Deny" --output json
```

### By Compliance Framework

```bash
# Find NIST initiatives
xpolicy initiative list --search "NIST" --output json

# Find ISO initiatives
xpolicy initiative list --search "ISO" --output json

# Find CIS initiatives
xpolicy initiative list --search "CIS" --output json
```

## Error Handling

### Invalid UUID

If `xpolicy policy get <uuid>` returns an error, the UUID does not exist in the Azure Policy catalog. Possible causes:

- Typo in the UUID
- The policy is a custom policy (not built-in) and won't be in the catalog
- The policy was removed by Microsoft

**Action:** Search by keyword to find the correct policy:
```bash
xpolicy policy list --search "<keywords from description>" --output json --limit 10
```

### Deprecated Policy

If the returned policy has `"deprecated": true`:

- Flag this to the user
- Search for a replacement policy with similar keywords
- Check if Microsoft provides a successor in the policy metadata

### Preview Policy

If `"preview": true`:

- Inform the user that the policy is in preview
- Preview policies may change or be removed
- Consider whether a GA alternative exists
