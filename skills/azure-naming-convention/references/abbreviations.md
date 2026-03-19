# Azure Resource Abbreviations

Complete list of Azure resource type abbreviations based on the Microsoft Cloud Adoption Framework (CAF).

**Source:** https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations

## AI + Machine Learning

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| AI Search | `srch` | `Microsoft.Search/searchServices` | `srch-<workload>-<env>` | `srch-navigator-prod` |
| Foundry Tools (multi-service) | `ais` | `Microsoft.CognitiveServices/accounts` | `ais-<workload>-<env>` | `ais-navigator-prod` |
| Foundry account | `aif` | `Microsoft.CognitiveServices/accounts` | `aif-<workload>-<env>` | `aif-navigator-prod` |
| Foundry account project | `proj` | `Microsoft.CognitiveServices/accounts/projects` | `proj-<workload>-<env>` | `proj-navigator-prod` |
| Foundry hub | `hub` | `Microsoft.MachineLearningServices/workspaces` | `hub-<workload>-<env>` | `hub-navigator-prod` |
| Foundry hub project | `proj` | `Microsoft.MachineLearningServices/workspaces` | `proj-<workload>-<env>` | `proj-navigator-prod` |
| Azure AI Video Indexer | `avi` | `Microsoft.VideoIndexer/accounts` | `avi-<workload>-<env>` | `avi-navigator-prod` |
| Azure Machine Learning workspace | `mlw` | `Microsoft.MachineLearningServices/workspaces` | `mlw-<workload>-<env>` | `mlw-navigator-prod` |
| Azure OpenAI Service | `oai` | `Microsoft.CognitiveServices/accounts` | `oai-<workload>-<env>` | `oai-navigator-prod` |
| Bot service | `bot` | `Microsoft.BotService/botServices` | `bot-<workload>-<env>` | `bot-navigator-prod` |
| Computer vision | `cv` | `Microsoft.CognitiveServices/accounts` | `cv-<workload>-<env>` | `cv-navigator-prod` |
| Content moderator | `cm` | `Microsoft.CognitiveServices/accounts` | `cm-<workload>-<env>` | `cm-navigator-prod` |
| Content safety | `cs` | `Microsoft.CognitiveServices/accounts` | `cs-<workload>-<env>` | `cs-navigator-prod` |
| Custom vision (prediction) | `cstv` | `Microsoft.CognitiveServices/accounts` | `cstv-<workload>-<env>` | `cstv-navigator-prod` |
| Custom vision (training) | `cstvt` | `Microsoft.CognitiveServices/accounts` | `cstvt-<workload>-<env>` | `cstvt-navigator-prod` |
| Document intelligence | `di` | `Microsoft.CognitiveServices/accounts` | `di-<workload>-<env>` | `di-navigator-prod` |
| Face API | `face` | `Microsoft.CognitiveServices/accounts` | `face-<workload>-<env>` | `face-navigator-prod` |
| Health Insights | `hi` | `Microsoft.CognitiveServices/accounts` | `hi-<workload>-<env>` | `hi-navigator-prod` |
| Immersive reader | `ir` | `Microsoft.CognitiveServices/accounts` | `ir-<workload>-<env>` | `ir-navigator-prod` |
| Language service | `lang` | `Microsoft.CognitiveServices/accounts` | `lang-<workload>-<env>` | `lang-navigator-prod` |
| Speech service | `spch` | `Microsoft.CognitiveServices/accounts` | `spch-<workload>-<env>` | `spch-navigator-prod` |
| Translator | `trsl` | `Microsoft.CognitiveServices/accounts` | `trsl-<workload>-<env>` | `trsl-navigator-prod` |

## Analytics and IoT

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Azure Analysis Services server | `as` | `Microsoft.AnalysisServices/servers` | `as-<workload>-<env>` | `as-navigator-prod` |
| Azure Databricks Access Connector | `dbac` | `Microsoft.Databricks/workspaces/accessConnectors` | `dbac-<workload>-<env>` | `dbac-navigator-prod` |
| Azure Databricks workspace | `dbw` | `Microsoft.Databricks/workspaces` | `dbw-<workload>-<env>` | `dbw-navigator-prod` |
| Azure Data Explorer cluster | `dec` | `Microsoft.Kusto/clusters` | `dec-<workload>-<env>` | `dec-navigator-prod` |
| Azure Data Explorer cluster database | `dedb` | `Microsoft.Kusto/clusters/databases` | `dedb-<workload>-<env>` | `dedb-navigator-prod` |
| Azure Data Factory | `adf` | `Microsoft.DataFactory/factories` | `adf-<workload>-<env>` | `adf-navigator-prod` |
| Azure Digital Twin instance | `dt` | `Microsoft.DigitalTwins/digitalTwinsInstances` | `dt-<workload>-<env>` | `dt-navigator-prod` |
| Azure Stream Analytics | `asa` | `Microsoft.StreamAnalytics/cluster` | `asa-<workload>-<env>` | `asa-navigator-prod` |
| Azure Synapse Analytics private link hub | `synplh` | `Microsoft.Synapse/privateLinkHubs` | `synplh-<workload>-<env>` | `synplh-navigator-prod` |
| Azure Synapse Analytics SQL Dedicated Pool | `syndp` | `Microsoft.Synapse/workspaces/sqlPools` | `syndp-<workload>-<env>` | `syndp-navigator-prod` |
| Azure Synapse Analytics Spark Pool | `synsp` | `Microsoft.Synapse/workspaces/bigDataPools` | `synsp-<workload>-<env>` | `synsp-navigator-prod` |
| Azure Synapse Analytics workspaces | `synw` | `Microsoft.Synapse/workspaces` | `synw-<workload>-<env>` | `synw-navigator-prod` |
| Data Lake Store account | `dls` | `Microsoft.DataLakeStore/accounts` | `dls<workload><env>` | `dlsnavigatorprod` |
| Event Hubs namespace | `evhns` | `Microsoft.EventHub/namespaces` | `evhns-<workload>-<env>` | `evhns-navigator-prod` |
| Event hub | `evh` | `Microsoft.EventHub/namespaces/eventHubs` | `evh-<workload>-<env>` | `evh-navigator-prod` |
| Event Grid domain | `evgd` | `Microsoft.EventGrid/domains` | `evgd-<workload>-<env>` | `evgd-navigator-prod` |
| Event Grid namespace | `evgns` | `Microsoft.EventGrid/namespaces` | `evgns-<workload>-<env>` | `evgns-navigator-prod` |
| Event Grid subscription | `evgs` | `Microsoft.EventGrid/eventSubscriptions` | `evgs-<workload>-<env>` | `evgs-navigator-prod` |
| Event Grid topic | `evgt` | `Microsoft.EventGrid/domains/topics` | `evgt-<workload>-<env>` | `evgt-navigator-prod` |
| Event Grid system topic | `egst` | `Microsoft.EventGrid/systemTopics` | `egst-<workload>-<env>` | `egst-navigator-prod` |
| Fabric Capacity | `fc` | `Microsoft.Fabric/capacities` | `fc-<workload>-<env>` | `fc-navigator-prod` |
| HDInsight - Hadoop cluster | `hadoop` | `Microsoft.HDInsight/clusters` | `hadoop-<workload>-<env>` | `hadoop-navigator-prod` |
| HDInsight - HBase cluster | `hbase` | `Microsoft.HDInsight/clusters` | `hbase-<workload>-<env>` | `hbase-navigator-prod` |
| HDInsight - Kafka cluster | `kafka` | `Microsoft.HDInsight/clusters` | `kafka-<workload>-<env>` | `kafka-navigator-prod` |
| HDInsight - Spark cluster | `spark` | `Microsoft.HDInsight/clusters` | `spark-<workload>-<env>` | `spark-navigator-prod` |
| HDInsight - Storm cluster | `storm` | `Microsoft.HDInsight/clusters` | `storm-<workload>-<env>` | `storm-navigator-prod` |
| HDInsight - ML Services cluster | `mls` | `Microsoft.HDInsight/clusters` | `mls-<workload>-<env>` | `mls-navigator-prod` |
| IoT hub | `iot` | `Microsoft.Devices/IotHubs` | `iot-<workload>-<env>` | `iot-navigator-prod` |
| Provisioning services | `provs` | `Microsoft.Devices/provisioningServices` | `provs-<workload>-<env>` | `provs-navigator-prod` |
| Provisioning services certificate | `pcert` | `Microsoft.Devices/provisioningServices/certificates` | `pcert-<workload>-<env>` | `pcert-navigator-prod` |
| Power BI Embedded | `pbi` | `Microsoft.PowerBIDedicated/capacities` | `pbi-<workload>-<env>` | `pbi-navigator-prod` |
| Time Series Insights environment | `tsi` | `Microsoft.TimeSeriesInsights/environments` | `tsi-<workload>-<env>` | `tsi-navigator-prod` |

## Compute and Web

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| App Service environment | `ase` | `Microsoft.Web/hostingEnvironments` | `ase-<workload>-<env>` | `ase-navigator-prod` |
| App Service plan | `asp` | `Microsoft.Web/serverFarms` | `asp-<workload>-<env>` | `asp-navigator-prod` |
| Azure Load Testing instance | `lt` | `Microsoft.LoadTestService/loadTests` | `lt-<workload>-<env>` | `lt-navigator-prod` |
| Availability set | `avail` | `Microsoft.Compute/availabilitySets` | `avail-<workload>-<env>` | `avail-navigator-prod` |
| Azure Arc enabled server | `arcs` | `Microsoft.HybridCompute/machines` | `arcs-<workload>-<env>` | `arcs-navigator-prod` |
| Azure Arc enabled Kubernetes cluster | `arck` | `Microsoft.Kubernetes/connectedClusters` | `arck-<workload>-<env>` | `arck-navigator-prod` |
| Azure Arc private link scope | `pls` | `Microsoft.HybridCompute/privateLinkScopes` | `pls-<workload>-<env>` | `pls-navigator-prod` |
| Azure Arc gateway | `arcgw` | `Microsoft.HybridCompute/gateways` | `arcgw-<workload>-<env>` | `arcgw-navigator-prod` |
| Batch accounts | `ba` | `Microsoft.Batch/batchAccounts` | `ba-<workload>-<env>` | `ba-navigator-prod` |
| Cloud service | `cld` | `Microsoft.Compute/cloudServices` | `cld-<workload>-<env>` | `cld-navigator-prod` |
| Communication Services | `acs` | `Microsoft.Communication/communicationServices` | `acs-<workload>-<env>` | `acs-navigator-prod` |
| Disk encryption set | `des` | `Microsoft.Compute/diskEncryptionSets` | `des-<workload>-<env>` | `des-navigator-prod` |
| Function app | `func` | `Microsoft.Web/sites` | `func-<workload>-<env>-<###>` | `func-navigator-prod-001` |
| Gallery | `gal` | `Microsoft.Compute/galleries` | `gal-<workload>-<env>` | `gal-navigator-prod` |
| Hosting environment | `host` | `Microsoft.Web/hostingEnvironments` | `host-<workload>-<env>` | `host-navigator-prod` |
| Image template | `it` | `Microsoft.VirtualMachineImages/imageTemplates` | `it-<workload>-<env>` | `it-navigator-prod` |
| Managed disk (OS) | `osdisk` | `Microsoft.Compute/disks` | `osdisk-<workload>-<env>-<###>` | `osdisk-navigator-prod-001` |
| Managed disk (data) | `disk` | `Microsoft.Compute/disks` | `disk-<workload>-<env>-<###>` | `disk-navigator-prod-001` |
| Notification Hubs | `ntf` | `Microsoft.NotificationHubs/namespaces/notificationHubs` | `ntf-<workload>-<env>` | `ntf-navigator-prod` |
| Notification Hubs namespace | `ntfns` | `Microsoft.NotificationHubs/namespaces` | `ntfns-<workload>-<env>` | `ntfns-navigator-prod` |
| Proximity placement group | `ppg` | `Microsoft.Compute/proximityPlacementGroups` | `ppg-<workload>-<env>` | `ppg-navigator-prod` |
| Restore point collection | `rpc` | `Microsoft.Compute/restorePointCollections` | `rpc-<workload>-<env>` | `rpc-navigator-prod` |
| Snapshot | `snap` | `Microsoft.Compute/snapshots` | `snap-<workload>-<env>-<###>` | `snap-navigator-prod-001` |
| Static web app | `stapp` | `Microsoft.Web/staticSites` | `stapp-<workload>-<env>` | `stapp-navigator-prod` |
| Virtual machine | `vm` | `Microsoft.Compute/virtualMachines` | `vm-<workload>-<env>-<###>` | `vm-sql-prod-001` |
| Virtual machine scale set | `vmss` | `Microsoft.Compute/virtualMachineScaleSets` | `vmss-<workload>-<env>-<###>` | `vmss-navigator-prod-001` |
| VM maintenance configuration | `mc` | `Microsoft.Maintenance/maintenanceConfigurations` | `mc-<workload>-<env>` | `mc-navigator-prod` |
| VM storage account | `stvm` | `Microsoft.Storage/storageAccounts` | `stvm<workload><###>` | `stvmnavigator001` |
| Web app | `app` | `Microsoft.Web/sites` | `app-<workload>-<env>-<###>` | `app-navigator-prod-001` |

## Containers

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| AKS cluster | `aks` | `Microsoft.ContainerService/managedClusters` | `aks-<workload>-<env>-<###>` | `aks-navigator-prod-001` |
| AKS system node pool | `npsystem` | `Microsoft.ContainerService/managedClusters/agentPools` | `npsystem` | `npsystem` |
| AKS user node pool | `np` | `Microsoft.ContainerService/managedClusters/agentPools` | `np<workload>` | `npnavigator` |
| Container apps | `ca` | `Microsoft.App/containerApps` | `ca-<workload>-<env>` | `ca-navigator-prod` |
| Container apps environment | `cae` | `Microsoft.App/managedEnvironments` | `cae-<workload>-<env>` | `cae-navigator-prod` |
| Container apps job | `caj` | `Microsoft.App/jobs` | `caj-<workload>-<env>` | `caj-navigator-prod` |
| Container registry | `cr` | `Microsoft.ContainerRegistry/registries` | `cr<workload><env><###>` | `crnavigatorprod001` |
| Container instance | `ci` | `Microsoft.ContainerInstance/containerGroups` | `ci-<workload>-<env>-<###>` | `ci-navigator-prod-001` |
| Service Fabric cluster | `sf` | `Microsoft.ServiceFabric/clusters` | `sf-<workload>-<env>` | `sf-navigator-prod` |
| Service Fabric managed cluster | `sfmc` | `Microsoft.ServiceFabric/managedClusters` | `sfmc-<workload>-<env>` | `sfmc-navigator-prod` |

## Databases

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Azure Cosmos DB database | `cosmos` | `Microsoft.DocumentDB/databaseAccounts/sqlDatabases` | `cosmos-<workload>-<env>` | `cosmos-navigator-prod` |
| Azure Cosmos DB for Apache Cassandra | `coscas` | `Microsoft.DocumentDB/databaseAccounts` | `coscas-<workload>-<env>` | `coscas-navigator-prod` |
| Azure Cosmos DB for MongoDB | `cosmon` | `Microsoft.DocumentDB/databaseAccounts` | `cosmon-<workload>-<env>` | `cosmon-navigator-prod` |
| Azure Cosmos DB for NoSQL | `cosno` | `Microsoft.DocumentDb/databaseAccounts` | `cosno-<workload>-<env>` | `cosno-navigator-prod` |
| Azure Cosmos DB for Table | `costab` | `Microsoft.DocumentDb/databaseAccounts` | `costab-<workload>-<env>` | `costab-navigator-prod` |
| Azure Cosmos DB for Apache Gremlin | `cosgrm` | `Microsoft.DocumentDb/databaseAccounts` | `cosgrm-<workload>-<env>` | `cosgrm-navigator-prod` |
| Azure Cosmos DB PostgreSQL cluster | `cospos` | `Microsoft.DBforPostgreSQL/serverGroupsv2` | `cospos-<workload>-<env>` | `cospos-navigator-prod` |
| Azure Managed Redis | `amr` | `Microsoft.Cache/RedisEnterprise` | `amr-<workload>-<env>` | `amr-navigator-prod` |
| Azure SQL Database server | `sql` | `Microsoft.Sql/servers` | `sql-<workload>-<env>` | `sql-navigator-prod` |
| Azure SQL database | `sqldb` | `Microsoft.Sql/servers/databases` | `sqldb-<workload>-<env>` | `sqldb-users-prod` |
| Azure SQL Elastic Job agent | `sqlja` | `Microsoft.Sql/servers/jobAgents` | `sqlja-<workload>-<env>` | `sqlja-navigator-prod` |
| Azure SQL Elastic Pool | `sqlep` | `Microsoft.Sql/servers/elasticpool` | `sqlep-<workload>-<env>` | `sqlep-navigator-prod` |
| MySQL database | `mysql` | `Microsoft.DBforMySQL/servers` | `mysql-<workload>-<env>` | `mysql-navigator-prod` |
| PostgreSQL database | `psql` | `Microsoft.DBforPostgreSQL/servers` | `psql-<workload>-<env>` | `psql-navigator-prod` |
| SQL Managed Instance | `sqlmi` | `Microsoft.Sql/managedInstances` | `sqlmi-<workload>-<env>` | `sqlmi-navigator-prod` |

## Developer Tools

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| App Configuration store | `appcs` | `Microsoft.AppConfiguration/configurationStores` | `appcs-<workload>-<env>` | `appcs-navigator-prod` |
| Maps account | `map` | `Microsoft.Maps/accounts` | `map-<workload>-<env>` | `map-navigator-prod` |
| SignalR | `sigr` | `Microsoft.SignalRService/SignalR` | `sigr-<workload>-<env>` | `sigr-navigator-prod` |
| WebPubSub | `wps` | `Microsoft.SignalRService/webPubSub` | `wps-<workload>-<env>` | `wps-navigator-prod` |

## DevOps

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Azure Managed Grafana | `amg` | `Microsoft.Dashboard/grafana` | `amg-<workload>-<env>` | `amg-navigator-prod` |
| Managed DevOps Pools | `mdp` | `Microsoft.DevOpsInfrastructure/pools` | `mdp-<workload>-<env>` | `mdp-navigator-prod` |

## Integration

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| API management service instance | `apim` | `Microsoft.ApiManagement/service` | `apim-<workload>-<env>` | `apim-navigator-prod` |
| Integration account | `ia` | `Microsoft.Logic/integrationAccounts` | `ia-<workload>-<env>` | `ia-navigator-prod` |
| Logic app | `logic` | `Microsoft.Logic/workflows` | `logic-<workload>-<env>` | `logic-navigator-prod` |
| Service Bus namespace | `sbns` | `Microsoft.ServiceBus/namespaces` | `sbns-<workload>-<env>` | `sbns-navigator-prod` |
| Service Bus queue | `sbq` | `Microsoft.ServiceBus/namespaces/queues` | `sbq-<workload>` | `sbq-navigator` |
| Service Bus topic | `sbt` | `Microsoft.ServiceBus/namespaces/topics` | `sbt-<workload>` | `sbt-navigator` |
| Service Bus topic subscription | `sbts` | `Microsoft.ServiceBus/namespaces/topics/subscriptions` | `sbts-<workload>` | `sbts-navigator` |

## Management and Governance

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Automation account | `aa` | `Microsoft.Automation/automationAccounts` | `aa-<workload>-<env>` | `aa-navigator-prod` |
| Application Insights | `appi` | `Microsoft.Insights/components` | `appi-<workload>-<env>` | `appi-navigator-prod` |
| Azure Monitor action group | `ag` | `Microsoft.Insights/actionGroups` | `ag-<workload>-<env>` | `ag-navigator-prod` |
| Azure Monitor data collection rule | `dcr` | `Microsoft.Insights/dataCollectionRules` | `dcr-<workload>-<env>` | `dcr-navigator-prod` |
| Azure Monitor alert processing rule | `apr` | `Microsoft.AlertsManagement/actionRules` | `apr-<workload>-<env>` | `apr-navigator-prod` |
| Data collection endpoint | `dce` | `Microsoft.Insights/dataCollectionEndpoints` | `dce-<workload>-<env>` | `dce-navigator-prod` |
| Deployment scripts | `script` | `Microsoft.Resources/deploymentScripts` | `script-<workload>-<env>` | `script-navigator-prod` |
| Log Analytics workspace | `log` | `Microsoft.OperationalInsights/workspaces` | `log-<workload>-<env>` | `log-navigator-prod` |
| Log Analytics query packs | `pack` | `Microsoft.OperationalInsights/querypacks` | `pack-<workload>-<env>` | `pack-navigator-prod` |
| Management group | `mg` | `Microsoft.Management/managementGroups` | `mg-<purpose>` | `mg-platform` |
| Microsoft Purview instance | `pview` | `Microsoft.Purview/accounts` | `pview-<workload>-<env>` | `pview-navigator-prod` |
| Resource group | `rg` | `Microsoft.Resources/resourceGroups` | `rg-<workload>-<env>` | `rg-webapp-prod` |
| Template specs name | `ts` | `Microsoft.Resources/templateSpecs` | `ts-<workload>-<env>` | `ts-navigator-prod` |

## Migration

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Azure Migrate project | `migr` | `Microsoft.Migrate/assessmentProjects` | `migr-<workload>-<env>` | `migr-navigator-prod` |
| Database Migration Service instance | `dms` | `Microsoft.DataMigration/services` | `dms-<workload>-<env>` | `dms-navigator-prod` |
| Recovery Services vault | `rsv` | `Microsoft.RecoveryServices/vaults` | `rsv-<workload>-<env>` | `rsv-navigator-prod` |

## Networking

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Application gateway | `agw` | `Microsoft.Network/applicationGateways` | `agw-<workload>-<env>-<###>` | `agw-navigator-prod-001` |
| Application security group (ASG) | `asg` | `Microsoft.Network/applicationSecurityGroups` | `asg-<workload>-<env>` | `asg-navigator-prod` |
| CDN profile | `cdnp` | `Microsoft.Cdn/profiles` | `cdnp-<workload>-<env>` | `cdnp-navigator-prod` |
| CDN endpoint | `cdne` | `Microsoft.Cdn/profiles/endpoints` | `cdne-<workload>-<env>` | `cdne-navigator-prod` |
| Connections | `con` | `Microsoft.Network/connections` | `con-<workload>-<env>` | `con-navigator-prod` |
| DNS forwarding ruleset | `dnsfrs` | `Microsoft.Network/dnsForwardingRulesets` | `dnsfrs-<workload>-<env>` | `dnsfrs-navigator-prod` |
| DNS private resolver | `dnspr` | `Microsoft.Network/dnsResolvers` | `dnspr-<workload>-<env>` | `dnspr-navigator-prod` |
| DNS private resolver inbound endpoint | `in` | `Microsoft.Network/dnsResolvers/inboundEndpoints` | `in-<workload>-<env>` | `in-navigator-prod` |
| DNS private resolver outbound endpoint | `out` | `Microsoft.Network/dnsResolvers/outboundEndpoints` | `out-<workload>-<env>` | `out-navigator-prod` |
| Firewall | `afw` | `Microsoft.Network/azureFirewalls` | `afw-<workload>-<env>` | `afw-navigator-prod` |
| Firewall policy | `afwp` | `Microsoft.Network/firewallPolicies` | `afwp-<workload>-<env>` | `afwp-navigator-prod` |
| ExpressRoute circuit | `erc` | `Microsoft.Network/expressRouteCircuits` | `erc-<workload>-<env>` | `erc-navigator-prod` |
| ExpressRoute direct | `erd` | `Microsoft.Network/expressRoutePorts` | `erd-<workload>-<env>` | `erd-navigator-prod` |
| ExpressRoute gateway | `ergw` | `Microsoft.Network/virtualNetworkGateways` | `ergw-<workload>-<env>` | `ergw-navigator-prod` |
| Front Door (Standard/Premium) profile | `afd` | `Microsoft.Cdn/profiles` | `afd-<workload>-<env>` | `afd-navigator-prod` |
| Front Door (Standard/Premium) endpoint | `fde` | `Microsoft.Cdn/profiles/afdEndpoints` | `fde-<workload>-<env>` | `fde-navigator-prod` |
| Front Door firewall policy | `fdfp` | `Microsoft.Network/frontdoorWebApplicationFirewallPolicies` | `fdfp-<workload>-<env>` | `fdfp-navigator-prod` |
| Front Door (classic) | `afd` | `Microsoft.Network/frontDoors` | `afd-<workload>-<env>` | `afd-navigator-prod` |
| IP group | `ipg` | `Microsoft.Network/ipGroups` | `ipg-<workload>-<env>` | `ipg-navigator-prod` |
| Load balancer (internal) | `lbi` | `Microsoft.Network/loadBalancers` | `lbi-<workload>-<env>-<###>` | `lbi-navigator-prod-001` |
| Load balancer (external) | `lbe` | `Microsoft.Network/loadBalancers` | `lbe-<workload>-<env>-<###>` | `lbe-navigator-prod-001` |
| Load balancer rule | `rule` | `Microsoft.Network/loadBalancers/inboundNatRules` | `rule-<workload>-<env>` | `rule-navigator-prod` |
| Local network gateway | `lgw` | `Microsoft.Network/localNetworkGateways` | `lgw-<purpose>-<region>-<###>` | `lgw-shared-eastus2-001` |
| NAT gateway | `ng` | `Microsoft.Network/natGateways` | `ng-<workload>-<env>` | `ng-navigator-prod` |
| Network interface (NIC) | `nic` | `Microsoft.Network/networkInterfaces` | `nic-<##>-<vm name>-<purpose>-<###>` | `nic-01-vm-sql-prod-001` |
| Network security perimeter | `nsp` | `Microsoft.Network/networkSecurityPerimeters` | `nsp-<workload>-<env>` | `nsp-navigator-prod` |
| Network security group (NSG) | `nsg` | `Microsoft.Network/networkSecurityGroups` | `nsg-<policy or app>-<###>` | `nsg-weballow-001` |
| NSG security rules | `nsgsr` | `Microsoft.Network/networkSecurityGroups/securityRules` | `nsgsr-<description>-<###>` | `nsgsr-rdpallow-001` |
| Network Watcher | `nw` | `Microsoft.Network/networkWatchers` | `nw-<workload>-<env>` | `nw-navigator-prod` |
| Private Link | `pl` | `Microsoft.Network/privateLinkServices` | `pl-<workload>-<env>` | `pl-navigator-prod` |
| Private endpoint | `pep` | `Microsoft.Network/privateEndpoints` | `pep-<workload>-<env>` | `pep-navigator-prod` |
| Public IP address | `pip` | `Microsoft.Network/publicIPAddresses` | `pip-<vm or app>-<env>-<region>-<###>` | `pip-navigator-prod-westus-001` |
| Public IP address prefix | `ippre` | `Microsoft.Network/publicIPPrefixes` | `ippre-<workload>-<env>-<region>` | `ippre-navigator-prod-westus` |
| Route filter | `rf` | `Microsoft.Network/routeFilters` | `rf-<workload>-<env>` | `rf-navigator-prod` |
| Route server | `rtserv` | `Microsoft.Network/virtualHubs` | `rtserv-<workload>-<env>` | `rtserv-navigator-prod` |
| Route table | `rt` | `Microsoft.Network/routeTables` | `rt-<name>` | `rt-navigator` |
| Service endpoint policy | `se` | `Microsoft.Network/serviceEndPointPolicies` | `se-<workload>-<env>` | `se-navigator-prod` |
| Traffic Manager profile | `traf` | `Microsoft.Network/trafficManagerProfiles` | `traf-<workload>-<env>` | `traf-navigator-prod` |
| User defined route (UDR) | `udr` | `Microsoft.Network/routeTables/routes` | `udr-<workload>-<env>` | `udr-navigator-prod` |
| Virtual network | `vnet` | `Microsoft.Network/virtualNetworks` | `vnet-<purpose>-<region>-<###>` | `vnet-prod-westus-001` |
| Virtual network gateway | `vgw` | `Microsoft.Network/virtualNetworkGateways` | `vgw-<purpose>-<region>-<###>` | `vgw-shared-eastus2-001` |
| Virtual network manager | `vnm` | `Microsoft.Network/networkManagers` | `vnm-<workload>-<env>` | `vnm-navigator-prod` |
| Virtual network peering | `peer` | `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` | `peer-<vnet1>-to-<vnet2>` | `peer-prod-to-shared` |
| Virtual network subnet | `snet` | `Microsoft.Network/virtualNetworks/subnets` | `snet-<purpose>-<region>-<###>` | `snet-prod-westus-001` |
| Virtual WAN | `vwan` | `Microsoft.Network/virtualWans` | `vwan-<workload>-<env>` | `vwan-navigator-prod` |
| Virtual WAN Hub | `vhub` | `Microsoft.Network/virtualHubs` | `vhub-<workload>-<env>` | `vhub-navigator-prod` |

## Security

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Azure Bastion | `bas` | `Microsoft.Network/bastionHosts` | `bas-<workload>-<env>` | `bas-navigator-prod` |
| Key vault | `kv` | `Microsoft.KeyVault/vaults` | `kv-<workload>-<env>` | `kv-navigator-prod` |
| Key Vault Managed HSM | `kvmhsm` | `Microsoft.KeyVault/managedHSMs` | `kvmhsm-<workload>-<env>` | `kvmhsm-navigator-prod` |
| Managed identity | `id` | `Microsoft.ManagedIdentity/userAssignedIdentities` | `id-<app>-<env>-<region>-<###>` | `id-navigator-prod-eastus2-001` |
| SSH key | `sshkey` | `Microsoft.Compute/sshPublicKeys` | `sshkey-<workload>-<env>` | `sshkey-navigator-prod` |
| VPN Gateway | `vpng` | `Microsoft.Network/vpnGateways` | `vpng-<workload>-<env>` | `vpng-navigator-prod` |
| VPN connection | `vcn` | `Microsoft.Network/vpnGateways/vpnConnections` | `vcn-<from>-<region>-to-<to>-<region>` | `vcn-shared-eastus2-to-shared-westus` |
| VPN site | `vst` | `Microsoft.Network/vpnGateways/vpnSites` | `vst-<workload>-<env>` | `vst-navigator-prod` |
| WAF policy | `waf` | `Microsoft.Network/firewallPolicies` | `waf-<workload>-<env>` | `waf-navigator-prod` |
| WAF policy rule group | `wafrg` | `Microsoft.Network/firewallPolicies/ruleGroups` | `wafrg-<workload>-<env>` | `wafrg-navigator-prod` |

## Storage

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Backup Vault | `bvault` | `Microsoft.DataProtection/backupVaults` | `bvault-<workload>-<env>` | `bvault-navigator-prod` |
| Backup Vault policy | `bkpol` | `Microsoft.DataProtection/backupVaults/backupPolicies` | `bkpol-<workload>-<env>` | `bkpol-navigator-prod` |
| File share | `share` | `Microsoft.Storage/storageAccounts/fileServices/shares` | `share-<workload>-<env>` | `share-navigator-prod` |
| Storage account | `st` | `Microsoft.Storage/storageAccounts` | `st<workload><###>` | `stnavigatordata001` |
| Storage Sync Service | `sss` | `Microsoft.StorageSync/storageSyncServices` | `sss-<workload>-<env>` | `sss-navigator-prod` |

## Virtual Desktop Infrastructure

| Resource | Abbreviation | Resource Provider Namespace | Naming Format | Example |
|----------|-------------|-----------------------------|---------------|---------|
| Virtual desktop host pool | `vdpool` | `Microsoft.DesktopVirtualization/hostPools` | `vdpool-<workload>-<env>` | `vdpool-navigator-prod` |
| Virtual desktop application group | `vdag` | `Microsoft.DesktopVirtualization/applicationGroups` | `vdag-<workload>-<env>` | `vdag-navigator-prod` |
| Virtual desktop workspace | `vdws` | `Microsoft.DesktopVirtualization/workspaces` | `vdws-<workload>-<env>` | `vdws-navigator-prod` |
| Virtual desktop scaling plan | `vdscaling` | `Microsoft.DesktopVirtualization/scalingPlans` | `vdscaling-<workload>-<env>` | `vdscaling-navigator-prod` |
