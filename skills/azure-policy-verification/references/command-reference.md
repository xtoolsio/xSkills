# xPolicy Command Reference

Complete CLI reference for the xpolicy tool. All commands are read-only and query a local SQLite database.

**Brand URL:** xtools.com/xpolicy
**Source:** github.com/xtoolsio/xTools (monorepo, under `xPolicy/`)
**Binary:** `xpolicy`

## Policy Commands

### `xpolicy policy get`

Look up a single policy by UUID or display name.

```bash
# By UUID
xpolicy policy get 06a78e20-9358-41c9-923c-fb736d382a4d --output json

# By display name
xpolicy policy get "Audit virtual machines without disaster recovery configured" --output json
```

**Flags:**

| Flag | Description |
|------|-------------|
| `-o, --output` | Output format: `auto`, `table`, `json`, `yaml` |

**JSON response fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Policy UUID |
| `name` | string | Display name |
| `description` | string | Full description |
| `version` | string | Semantic version (e.g., `1.0.0`) |
| `category` | string | Policy category (e.g., `Compute`, `Storage`) |
| `mode` | string | Policy mode (`All`, `Indexed`, etc.) |
| `type` | string | `BuiltIn`, `Static`, `ALZ`, `AMBA`, or `Community` |
| `preview` | boolean | Whether the policy is in preview |
| `deprecated` | boolean | Whether the policy is deprecated |
| `effect` | string | Default effect (e.g., `Audit`, `Deny`, `AuditIfNotExists`) |
| `effectAllowedValues` | string | Semicolon-separated list of allowed effects, or `n/a` |
| `rolesUsedCount` | number | Number of roles used by the policy |
| `usedInPolicySetCount` | number | Number of initiatives that include this policy |
| `usedInPolicySet` | string | Semicolon-separated list of initiatives using this policy |
| `dateAdded` | string | Date the policy was added to the catalog |
| `cloudEnvs` | string | Supported cloud environments (semicolon-separated) |
| `definition` | string | Full JSON policy definition (stringified) |

### `xpolicy policy list`

Search and filter policies.

```bash
xpolicy policy list [flags] --output json
```

**Flags:**

| Flag | Description | Example |
|------|-------------|---------|
| `--search <text>` | Full-text search (FTS5) across name and description | `--search "encryption at rest"` |
| `--category <name>` | Filter by category (case-insensitive) | `--category "Key Vault"` |
| `--effect <name>` | Filter by effect (case-insensitive) | `--effect "Deny"` |
| `--type <name>` | Filter by type: `BuiltIn`, `Static`, `ALZ`, `AMBA`, `Community` | `--type BuiltIn` |
| `--state <name>` | Filter by state: `ga`, `preview`, `deprecated` | `--state ga` |
| `--cloud <name>` | Filter by cloud: `AzureCloud`, `AzureUSGovernment` | `--cloud AzureUSGovernment` |
| `--mode <name>` | Filter by mode: `Indexed`, `All` | `--mode Indexed` |
| `--limit <n>` | Max number of results | `--limit 20` |
| `-o, --output` | Output format: `auto`, `table`, `json`, `yaml` | `--output json` |

**Examples:**

```bash
# Storage deny policies
xpolicy policy list --category "Storage" --effect "Deny" --output json

# GA built-in network policies
xpolicy policy list --type BuiltIn --state ga --search "network" --output json --limit 20

# All deprecated policies
xpolicy policy list --state deprecated --output json

# Key Vault audit policies
xpolicy policy list --category "Key Vault" --effect "Audit" --output json

# US Government cloud policies
xpolicy policy list --cloud "AzureUSGovernment" --category "Compute" --output json
```

### `xpolicy policy categories`

List all policy categories with counts.

```bash
xpolicy policy categories --output json
```

### `xpolicy policy effects`

List all policy effects with counts.

```bash
xpolicy policy effects --output json
```

### `xpolicy policy stats`

Summary statistics of the policy database.

```bash
xpolicy policy stats --output json
```

Returns total policies, breakdowns by type, effect, and category, plus preview and deprecated counts.

## Initiative Commands

### `xpolicy initiative get`

Look up a single initiative (policy set) by UUID or name. Returns the initiative details and all contained policies.

```bash
# By UUID
xpolicy initiative get 179d1daa-458f-4e47-8086-2a68d0d6c38f --output json

# By name
xpolicy initiative get "NIST SP 800-53 Rev. 5" --output json
```

**JSON response fields (initiative):**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Initiative UUID |
| `name` | string | Display name |
| `description` | string | Full description |
| `category` | string | Initiative category |
| `version` | string | Semantic version |
| `type` | string | `BuiltIn`, `ALZ`, or `AMBA` |
| `cloudEnvs` | string | Supported cloud environments |
| `azUSGov` | string | Whether available in US Government cloud |
| `azUSGovVersion` | string | Version available in US Government cloud |
| `policyCount` | number | Number of policies in the initiative |
| `policies` | array | Array of contained policy objects |
| `definition` | string | Full JSON initiative definition (stringified) |

**JSON response fields (each policy in `policies` array):**

| Field | Type | Description |
|-------|------|-------------|
| `initiativeId` | string | Parent initiative UUID |
| `referenceId` | string | Policy reference ID within the initiative |
| `policyId` | string | Policy UUID |
| `policyName` | string | Policy display name |
| `policyDescription` | string | Policy description |
| `policyCategory` | string | Policy category |
| `policyVersion` | string | Policy version |
| `policyMode` | string | Policy mode |
| `policyType` | string | Policy type |
| `policyPreview` | boolean | Whether the policy is in preview |
| `policyDeprecated` | boolean | Whether the policy is deprecated |
| `effectDefault` | string | Default effect |
| `effectAllowed` | string | Allowed effect values |

### `xpolicy initiative list`

Search and filter initiatives.

```bash
xpolicy initiative list [flags] --output json
```

**Flags:**

| Flag | Description | Example |
|------|-------------|---------|
| `--search <text>` | Full-text search across initiative name and description | `--search "NIST"` |
| `--category <name>` | Filter by category (case-insensitive) | `--category "Regulatory Compliance"` |
| `--cloud <name>` | Filter by cloud: `AzureCloud`, `AzureUSGovernment` | `--cloud AzureCloud` |
| `--type <name>` | Filter by type: `BuiltIn`, `ALZ`, `AMBA` | `--type ALZ` |
| `--limit <n>` | Max number of results | `--limit 10` |
| `-o, --output` | Output format: `auto`, `table`, `json`, `yaml` | `--output json` |

**Examples:**

```bash
# Regulatory compliance initiatives
xpolicy initiative list --category "Regulatory Compliance" --output json

# Search for CIS benchmarks
xpolicy initiative list --search "CIS" --output json

# Search for specific framework
xpolicy initiative list --search "ISO 27001" --output json

# Azure Landing Zone initiatives
xpolicy initiative list --type ALZ --output json
```

### `xpolicy initiative categories`

List all initiative categories with counts.

```bash
xpolicy initiative categories --output json
```

### `xpolicy initiative stats`

Summary statistics of the initiative database.

```bash
xpolicy initiative stats --output json
```

Returns total initiatives, total policy references, and breakdowns by type and category.

## Database Management

### `xpolicy update`

Check for and download the latest policy database.

```bash
xpolicy update --force
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--force` | Re-download even if the database is current |

The database auto-updates daily from DigitalOcean Spaces. Use `--force` only when the latest data is needed immediately.

### `xpolicy version`

Show version information including binary version, commit, build time, and database version.

```bash
xpolicy version
```

## Usage Notes

- **Always use `--output json`** — gives structured data for reliable parsing
- **Piped output is always JSON** — when xpolicy output is piped (not a TTY), it automatically outputs JSON even without `--output json`
- **xpolicy is read-only** — no Azure API calls, no modifications, safe to run anywhere
- **Case-insensitive filters** — category, effect, and type filters work regardless of case
- **FTS5 search** — the `--search` flag uses SQLite FTS5 full-text search; partial words and phrases work well
- **Extract UUIDs from resource IDs** — if code has `/providers/Microsoft.Authorization/policyDefinitions/06a78e20-...`, extract just the UUID: `06a78e20-9358-41c9-923c-fb736d382a4d`
- **Policy types** — policies include `BuiltIn`, `Static`, `ALZ`, `AMBA`, `Community`; initiatives include `BuiltIn`, `ALZ`, `AMBA`
