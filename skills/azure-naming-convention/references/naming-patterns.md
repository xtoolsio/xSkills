# Azure Naming Patterns for IaC

Detailed Infrastructure-as-Code examples showing how to apply CAF naming conventions in Terraform, OpenTofu, Bicep, ARM templates, and Pulumi.

## Terraform / OpenTofu

### Recommended Variables

Define these variables once and reuse across all resource names:

```hcl
variable "workload" {
  description = "Workload, application, or project name"
  type        = string
}

variable "environment" {
  description = "Deployment environment"
  type        = string
  validation {
    condition     = contains(["prod", "dev", "qa", "stage", "test"], var.environment)
    error_message = "Environment must be one of: prod, dev, qa, stage, test."
  }
}

variable "location" {
  description = "Azure region for resource deployment"
  type        = string
}

variable "location_short" {
  description = "Short Azure region name for naming (e.g., westus, eastus2, westeu)"
  type        = string
}

variable "instance" {
  description = "Zero-padded instance number"
  type        = string
  default     = "001"
}
```

### Management and Governance

```hcl
resource "azurerm_resource_group" "main" {
  name     = "rg-${var.workload}-${var.environment}"
  location = var.location
}

resource "azurerm_log_analytics_workspace" "main" {
  name                = "log-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku                 = "PerGB2018"
}

resource "azurerm_application_insights" "main" {
  name                = "appi-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  application_type    = "web"
  workspace_id        = azurerm_log_analytics_workspace.main.id
}
```

### Networking

```hcl
resource "azurerm_virtual_network" "main" {
  name                = "vnet-${var.workload}-${var.location_short}-${var.instance}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "app" {
  name                 = "snet-app-${var.location_short}-${var.instance}"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_subnet" "db" {
  name                 = "snet-db-${var.location_short}-${var.instance}"
  resource_group_name  = azurerm_resource_group.main.name
  virtual_network_name = azurerm_virtual_network.main.name
  address_prefixes     = ["10.0.2.0/24"]
}

resource "azurerm_network_security_group" "web" {
  name                = "nsg-weballow-001"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_public_ip" "main" {
  name                = "pip-${var.workload}-${var.environment}-${var.location_short}-${var.instance}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  allocation_method   = "Static"
  sku                 = "Standard"
}

resource "azurerm_lb" "external" {
  name                = "lbe-${var.workload}-${var.environment}-${var.instance}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku                 = "Standard"

  frontend_ip_configuration {
    name                 = "frontend"
    public_ip_address_id = azurerm_public_ip.main.id
  }
}

resource "azurerm_route_table" "main" {
  name                = "rt-${var.workload}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_nat_gateway" "main" {
  name                = "ng-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_private_endpoint" "main" {
  name                = "pep-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  subnet_id           = azurerm_subnet.app.id

  private_service_connection {
    name                           = "psc-${var.workload}-${var.environment}"
    is_manual_connection           = false
    # private_connection_resource_id = ...
  }
}

resource "azurerm_firewall" "main" {
  name                = "afw-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku_name            = "AZFW_VNet"
  sku_tier            = "Standard"
}

resource "azurerm_firewall_policy" "main" {
  name                = "afwp-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}
```

### Compute and Web

```hcl
resource "azurerm_network_interface" "vm" {
  name                = "nic-01-vm-${var.workload}-${var.environment}-${var.instance}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.app.id
    private_ip_address_allocation = "Dynamic"
  }
}

resource "azurerm_linux_virtual_machine" "main" {
  name                = "vm-${var.workload}-${var.environment}-${var.instance}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  size                = "Standard_B2s"

  network_interface_ids = [azurerm_network_interface.vm.id]

  os_disk {
    name                 = "osdisk-${var.workload}-${var.environment}-${var.instance}"
    caching              = "ReadWrite"
    storage_account_type = "Premium_LRS"
  }

  # source_image_reference { ... }
  # admin_ssh_key { ... }
}

resource "azurerm_managed_disk" "data" {
  name                 = "disk-${var.workload}-${var.environment}-${var.instance}"
  location             = azurerm_resource_group.main.location
  resource_group_name  = azurerm_resource_group.main.name
  storage_account_type = "Premium_LRS"
  create_option        = "Empty"
  disk_size_gb         = 128
}

resource "azurerm_service_plan" "main" {
  name                = "asp-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  os_type             = "Linux"
  sku_name            = "P1v3"
}

resource "azurerm_linux_web_app" "main" {
  name                = "app-${var.workload}-${var.environment}-${var.instance}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  service_plan_id     = azurerm_service_plan.main.id

  site_config {}
}

resource "azurerm_linux_function_app" "main" {
  name                       = "func-${var.workload}-${var.environment}-${var.instance}"
  resource_group_name        = azurerm_resource_group.main.name
  location                   = azurerm_resource_group.main.location
  service_plan_id            = azurerm_service_plan.main.id
  storage_account_name       = azurerm_storage_account.main.name
  storage_account_access_key = azurerm_storage_account.main.primary_access_key

  site_config {}
}

resource "azurerm_static_web_app" "main" {
  name                = "stapp-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
}

resource "azurerm_virtual_machine_scale_set" "main" {
  name                = "vmss-${var.workload}-${var.environment}-${var.instance}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  # ...
}
```

### Containers

```hcl
resource "azurerm_kubernetes_cluster" "main" {
  name                = "aks-${var.workload}-${var.environment}-${var.instance}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  dns_prefix          = "aks-${var.workload}-${var.environment}"

  default_node_pool {
    name       = "npsystem"
    node_count = 3
    vm_size    = "Standard_D4s_v5"
  }

  identity {
    type = "SystemAssigned"
  }
}

resource "azurerm_kubernetes_cluster_node_pool" "user" {
  name                  = "npworker"
  kubernetes_cluster_id = azurerm_kubernetes_cluster.main.id
  vm_size               = "Standard_D4s_v5"
  node_count            = 3
}

# No hyphens allowed for container registry
resource "azurerm_container_registry" "main" {
  name                = "cr${var.workload}${var.environment}${var.instance}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku                 = "Premium"
}

resource "azurerm_container_app_environment" "main" {
  name                = "cae-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_container_app" "main" {
  name                         = "ca-${var.workload}-${var.environment}"
  container_app_environment_id = azurerm_container_app_environment.main.id
  resource_group_name          = azurerm_resource_group.main.name

  template {
    container {
      name   = var.workload
      image  = "${azurerm_container_registry.main.login_server}/${var.workload}:latest"
      cpu    = 0.5
      memory = "1Gi"
    }
  }
}

resource "azurerm_container_group" "main" {
  name                = "ci-${var.workload}-${var.environment}-${var.instance}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  os_type             = "Linux"

  container {
    name   = var.workload
    image  = "${azurerm_container_registry.main.login_server}/${var.workload}:latest"
    cpu    = 1
    memory = 2
  }
}
```

### Storage (No Hyphens)

```hcl
# Storage account: alphanumeric only, 3-24 chars, lowercase
resource "azurerm_storage_account" "main" {
  name                     = "st${var.workload}${var.instance}"
  resource_group_name      = azurerm_resource_group.main.name
  location                 = azurerm_resource_group.main.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_share" "main" {
  name               = "share-${var.workload}-${var.environment}"
  storage_account_id = azurerm_storage_account.main.id
  quota              = 50
}

resource "azurerm_recovery_services_vault" "main" {
  name                = "rsv-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku                 = "Standard"
}

resource "azurerm_data_protection_backup_vault" "main" {
  name                = "bvault-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  datastore_type      = "VaultStore"
  redundancy          = "LocallyRedundant"
}
```

### Databases

```hcl
resource "azurerm_mssql_server" "main" {
  name                         = "sql-${var.workload}-${var.environment}"
  resource_group_name          = azurerm_resource_group.main.name
  location                     = azurerm_resource_group.main.location
  version                      = "12.0"
  administrator_login          = "sqladmin"
  administrator_login_password = var.sql_admin_password
}

resource "azurerm_mssql_database" "main" {
  name      = "sqldb-${var.workload}-${var.environment}"
  server_id = azurerm_mssql_server.main.id
  sku_name  = "S0"
}

resource "azurerm_mssql_elasticpool" "main" {
  name                = "sqlep-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  server_name         = azurerm_mssql_server.main.name

  per_database_settings {
    min_capacity = 0.25
    max_capacity = 4
  }

  sku {
    name     = "GP_Gen5"
    tier     = "GeneralPurpose"
    capacity = 4
    family   = "Gen5"
  }
}

resource "azurerm_cosmosdb_account" "main" {
  name                = "cosmos-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  offer_type          = "Standard"
  kind                = "GlobalDocumentDB"

  consistency_policy {
    consistency_level = "Session"
  }

  geo_location {
    location          = azurerm_resource_group.main.location
    failover_priority = 0
  }
}

resource "azurerm_mysql_flexible_server" "main" {
  name                = "mysql-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku_name            = "GP_Standard_D2ds_v4"
}

resource "azurerm_postgresql_flexible_server" "main" {
  name                = "psql-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku_name            = "GP_Standard_D2s_v3"
  version             = "15"
}

resource "azurerm_redis_enterprise_cluster" "main" {
  name                = "amr-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku_name            = "Enterprise_E10-2"
}
```

### Security

```hcl
resource "azurerm_key_vault" "main" {
  name                = "kv-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  tenant_id           = data.azurerm_client_config.current.tenant_id
  sku_name            = "standard"
}

resource "azurerm_user_assigned_identity" "main" {
  name                = "id-${var.workload}-${var.environment}-${var.location_short}-${var.instance}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_bastion_host" "main" {
  name                = "bas-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name

  ip_configuration {
    name                 = "configuration"
    subnet_id            = azurerm_subnet.bastion.id
    public_ip_address_id = azurerm_public_ip.bastion.id
  }
}

resource "azurerm_vpn_gateway" "main" {
  name                = "vpng-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  virtual_hub_id      = azurerm_virtual_hub.main.id
}
```

### AI + Machine Learning

```hcl
resource "azurerm_search_service" "main" {
  name                = "srch-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku                 = "standard"
}

resource "azurerm_cognitive_account" "openai" {
  name                = "oai-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  kind                = "OpenAI"
  sku_name            = "S0"
}

resource "azurerm_machine_learning_workspace" "main" {
  name                    = "mlw-${var.workload}-${var.environment}"
  location                = azurerm_resource_group.main.location
  resource_group_name     = azurerm_resource_group.main.name
  application_insights_id = azurerm_application_insights.main.id
  key_vault_id            = azurerm_key_vault.main.id
  storage_account_id      = azurerm_storage_account.main.id

  identity {
    type = "SystemAssigned"
  }
}
```

### Analytics and IoT

```hcl
resource "azurerm_data_factory" "main" {
  name                = "adf-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_databricks_workspace" "main" {
  name                = "dbw-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku                 = "premium"
}

resource "azurerm_eventhub_namespace" "main" {
  name                = "evhns-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku                 = "Standard"
}

resource "azurerm_eventhub" "main" {
  name              = "evh-${var.workload}-${var.environment}"
  namespace_id      = azurerm_eventhub_namespace.main.id
  partition_count   = 2
  message_retention = 1
}

resource "azurerm_iothub" "main" {
  name                = "iot-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location

  sku {
    name     = "S1"
    capacity = 1
  }
}

resource "azurerm_stream_analytics_job" "main" {
  name                = "asa-${var.workload}-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  streaming_units     = 3

  transformation_query = "SELECT * INTO [output] FROM [input]"
}

# Data Lake: alphanumeric only, no hyphens
resource "azurerm_data_lake_store" "main" {
  name                = "dls${var.workload}${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
}
```

### Integration

```hcl
resource "azurerm_api_management" "main" {
  name                = "apim-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  publisher_name      = var.publisher_name
  publisher_email     = var.publisher_email
  sku_name            = "Developer_1"
}

resource "azurerm_servicebus_namespace" "main" {
  name                = "sbns-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  sku                 = "Standard"
}

resource "azurerm_servicebus_queue" "main" {
  name         = "sbq-${var.workload}"
  namespace_id = azurerm_servicebus_namespace.main.id
}

resource "azurerm_servicebus_topic" "main" {
  name         = "sbt-${var.workload}"
  namespace_id = azurerm_servicebus_namespace.main.id
}

resource "azurerm_logic_app_workflow" "main" {
  name                = "logic-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}
```

### Virtual Desktop Infrastructure

```hcl
resource "azurerm_virtual_desktop_host_pool" "main" {
  name                = "vdpool-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  type                = "Pooled"
  load_balancer_type  = "BreadthFirst"
}

resource "azurerm_virtual_desktop_application_group" "main" {
  name                = "vdag-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  type                = "Desktop"
  host_pool_id        = azurerm_virtual_desktop_host_pool.main.id
}

resource "azurerm_virtual_desktop_workspace" "main" {
  name                = "vdws-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_virtual_desktop_scaling_plan" "main" {
  name                = "vdscaling-${var.workload}-${var.environment}"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  time_zone           = "Eastern Standard Time"
}
```

## Bicep

```bicep
param workload string
param environment string
param location string = resourceGroup().location
param locationShort string
param instance string = '001'

resource rg 'Microsoft.Resources/resourceGroups@2023-07-01' = {
  name: 'rg-${workload}-${environment}'
  location: location
}

resource vnet 'Microsoft.Network/virtualNetworks@2023-11-01' = {
  name: 'vnet-${workload}-${locationShort}-${instance}'
  location: location
  properties: {
    addressSpace: { addressPrefixes: ['10.0.0.0/16'] }
  }
}

// No hyphens for storage accounts
resource st 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'st${workload}${instance}'
  location: location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
}

resource kv 'Microsoft.KeyVault/vaults@2023-07-01' = {
  name: 'kv-${workload}-${environment}'
  location: location
  properties: {
    sku: { family: 'A', name: 'standard' }
    tenantId: subscription().tenantId
  }
}
```

## ARM Templates

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0",
  "parameters": {
    "workload": { "type": "string" },
    "environment": { "type": "string" },
    "locationShort": { "type": "string" },
    "instance": { "type": "string", "defaultValue": "001" }
  },
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2023-11-01",
      "name": "[concat('vnet-', parameters('workload'), '-', parameters('locationShort'), '-', parameters('instance'))]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": ["10.0.0.0/16"] }
      }
    },
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2023-05-01",
      "name": "[concat('st', parameters('workload'), parameters('instance'))]",
      "location": "[resourceGroup().location]",
      "sku": { "name": "Standard_LRS" },
      "kind": "StorageV2"
    }
  ]
}
```

## Pulumi (TypeScript)

```typescript
import * as azure from "@pulumi/azure-native";

const config = new pulumi.Config();
const workload = config.require("workload");
const environment = config.require("environment");
const locationShort = config.require("locationShort");
const instance = config.get("instance") || "001";

const rg = new azure.resources.ResourceGroup("main", {
  resourceGroupName: `rg-${workload}-${environment}`,
});

const vnet = new azure.network.VirtualNetwork("main", {
  virtualNetworkName: `vnet-${workload}-${locationShort}-${instance}`,
  resourceGroupName: rg.name,
  addressSpace: { addressPrefixes: ["10.0.0.0/16"] },
});

// No hyphens for storage accounts
const sa = new azure.storage.StorageAccount("main", {
  accountName: `st${workload}${instance}`,
  resourceGroupName: rg.name,
  sku: { name: "Standard_LRS" },
  kind: "StorageV2",
});

const kv = new azure.keyvault.Vault("main", {
  vaultName: `kv-${workload}-${environment}`,
  resourceGroupName: rg.name,
  properties: {
    sku: { family: "A", name: "standard" },
    tenantId: azure.authorization.getClientConfig().then(c => c.tenantId),
  },
});
```
