# Terraform File Naming Patterns — Complete Reference

Comprehensive reference for Obay's Terraform Naming Convention (OTN). One block per file, named by block type and identifier.

## Resource Blocks

**Pattern:** `resource.<provider_resource>.<name>.tf`

The filename includes the full provider-prefixed resource type and the user-assigned block name.

### AWS Examples

| Resource Type | Block Name | Filename |
|---------------|-----------|----------|
| `aws_instance` | `web` | `resource.aws_instance.web.tf` |
| `aws_instance` | `web-server` | `resource.aws_instance.web-server.tf` |
| `aws_security_group` | `web` | `resource.aws_security_group.web.tf` |
| `aws_s3_bucket` | `logs` | `resource.aws_s3_bucket.logs.tf` |
| `aws_iam_role` | `lambda-exec` | `resource.aws_iam_role.lambda-exec.tf` |
| `aws_vpc` | `main` | `resource.aws_vpc.main.tf` |
| `aws_subnet` | `public` | `resource.aws_subnet.public.tf` |
| `aws_subnet` | `private` | `resource.aws_subnet.private.tf` |
| `aws_internet_gateway` | `main` | `resource.aws_internet_gateway.main.tf` |
| `aws_lb` | `web` | `resource.aws_lb.web.tf` |
| `aws_rds_instance` | `primary` | `resource.aws_rds_instance.primary.tf` |
| `aws_lambda_function` | `processor` | `resource.aws_lambda_function.processor.tf` |
| `aws_ecs_cluster` | `main` | `resource.aws_ecs_cluster.main.tf` |
| `aws_ecs_service` | `api` | `resource.aws_ecs_service.api.tf` |
| `aws_cloudfront_distribution` | `cdn` | `resource.aws_cloudfront_distribution.cdn.tf` |
| `aws_route53_zone` | `primary` | `resource.aws_route53_zone.primary.tf` |
| `aws_sqs_queue` | `orders` | `resource.aws_sqs_queue.orders.tf` |
| `aws_sns_topic` | `alerts` | `resource.aws_sns_topic.alerts.tf` |

### Azure Examples

| Resource Type | Block Name | Filename |
|---------------|-----------|----------|
| `azurerm_resource_group` | `main` | `resource.azurerm_resource_group.main.tf` |
| `azurerm_virtual_network` | `sandbox-vnet` | `resource.azurerm_virtual_network.sandbox-vnet.tf` |
| `azurerm_subnet` | `web` | `resource.azurerm_subnet.web.tf` |
| `azurerm_network_security_group` | `web` | `resource.azurerm_network_security_group.web.tf` |
| `azurerm_public_ip` | `web` | `resource.azurerm_public_ip.web.tf` |
| `azurerm_storage_account` | `data` | `resource.azurerm_storage_account.data.tf` |
| `azurerm_key_vault` | `main` | `resource.azurerm_key_vault.main.tf` |
| `azurerm_kubernetes_cluster` | `main` | `resource.azurerm_kubernetes_cluster.main.tf` |
| `azurerm_linux_virtual_machine` | `web` | `resource.azurerm_linux_virtual_machine.web.tf` |
| `azurerm_cosmosdb_account` | `main` | `resource.azurerm_cosmosdb_account.main.tf` |
| `azurerm_mssql_server` | `main` | `resource.azurerm_mssql_server.main.tf` |
| `azurerm_mssql_database` | `app` | `resource.azurerm_mssql_database.app.tf` |
| `azurerm_container_registry` | `main` | `resource.azurerm_container_registry.main.tf` |
| `azurerm_application_insights` | `main` | `resource.azurerm_application_insights.main.tf` |
| `azurerm_log_analytics_workspace` | `main` | `resource.azurerm_log_analytics_workspace.main.tf` |

### GCP Examples

| Resource Type | Block Name | Filename |
|---------------|-----------|----------|
| `google_compute_instance` | `web` | `resource.google_compute_instance.web.tf` |
| `google_compute_network` | `main` | `resource.google_compute_network.main.tf` |
| `google_compute_subnetwork` | `public` | `resource.google_compute_subnetwork.public.tf` |
| `google_storage_bucket` | `assets` | `resource.google_storage_bucket.assets.tf` |
| `google_container_cluster` | `primary` | `resource.google_container_cluster.primary.tf` |
| `google_sql_database_instance` | `main` | `resource.google_sql_database_instance.main.tf` |
| `google_cloud_run_service` | `api` | `resource.google_cloud_run_service.api.tf` |
| `google_pubsub_topic` | `events` | `resource.google_pubsub_topic.events.tf` |
| `google_bigquery_dataset` | `analytics` | `resource.google_bigquery_dataset.analytics.tf` |
| `google_project_iam_member` | `editor` | `resource.google_project_iam_member.editor.tf` |

## Data Source Blocks

**Pattern:** `data.<provider_resource>.<name>.tf`

| Data Source Type | Block Name | Filename |
|------------------|-----------|----------|
| `aws_ami` | `ubuntu` | `data.aws_ami.ubuntu.tf` |
| `aws_caller_identity` | `current` | `data.aws_caller_identity.current.tf` |
| `aws_availability_zones` | `available` | `data.aws_availability_zones.available.tf` |
| `aws_region` | `current` | `data.aws_region.current.tf` |
| `aws_iam_policy_document` | `assume-role` | `data.aws_iam_policy_document.assume-role.tf` |
| `azurerm_resource_group` | `production-rg` | `data.azurerm_resource_group.production-rg.tf` |
| `azurerm_client_config` | `current` | `data.azurerm_client_config.current.tf` |
| `azurerm_subscription` | `current` | `data.azurerm_subscription.current.tf` |
| `google_project` | `current` | `data.google_project.current.tf` |
| `google_compute_zones` | `available` | `data.google_compute_zones.available.tf` |

## Variable Blocks

**Pattern:** `variable.<name>.tf`

| Variable Name | Filename |
|---------------|----------|
| `environment` | `variable.environment.tf` |
| `instance_type` | `variable.instance_type.tf` |
| `region` | `variable.region.tf` |
| `location` | `variable.location.tf` |
| `project_name` | `variable.project_name.tf` |
| `tags` | `variable.tags.tf` |
| `vpc_cidr` | `variable.vpc_cidr.tf` |
| `enable_monitoring` | `variable.enable_monitoring.tf` |
| `cluster_version` | `variable.cluster_version.tf` |
| `domain_name` | `variable.domain_name.tf` |

## Output Blocks

**Pattern:** `output.<name>.tf`

| Output Name | Filename |
|-------------|----------|
| `vpc_id` | `output.vpc_id.tf` |
| `public_ip` | `output.public_ip.tf` |
| `database_url` | `output.database_url.tf` |
| `cluster_endpoint` | `output.cluster_endpoint.tf` |
| `storage_account_name` | `output.storage_account_name.tf` |
| `load_balancer_dns` | `output.load_balancer_dns.tf` |
| `api_gateway_url` | `output.api_gateway_url.tf` |
| `ssh_private_key` | `output.ssh_private_key.tf` |

## Module Blocks

**Pattern:** `module.<name>.tf`

| Module Name | Filename |
|-------------|----------|
| `vpc` | `module.vpc.tf` |
| `database` | `module.database.tf` |
| `load_balancer` | `module.load_balancer.tf` |
| `monitoring` | `module.monitoring.tf` |
| `iam` | `module.iam.tf` |
| `dns` | `module.dns.tf` |
| `cdn` | `module.cdn.tf` |
| `networking` | `module.networking.tf` |

## Provider Blocks

**Pattern:** `provider.<name>.tf` or `provider.<name>.<alias>.tf`

| Provider | Alias | Filename |
|----------|-------|----------|
| `aws` | — | `provider.aws.tf` |
| `aws` | `us-east-1` | `provider.aws.us-east-1.tf` |
| `aws` | `eu-west-1` | `provider.aws.eu-west-1.tf` |
| `azurerm` | — | `provider.azurerm.tf` |
| `azurerm` | `production` | `provider.azurerm.production.tf` |
| `azurerm` | `management` | `provider.azurerm.management.tf` |
| `google` | — | `provider.google.tf` |
| `google` | `us-central1` | `provider.google.us-central1.tf` |
| `kubernetes` | — | `provider.kubernetes.tf` |
| `helm` | — | `provider.helm.tf` |
| `random` | — | `provider.random.tf` |
| `null` | — | `provider.null.tf` |
| `local` | — | `provider.local.tf` |

## Special Blocks

These blocks use fixed filenames — always a single file:

| Block Type | Filename | Contents |
|------------|----------|----------|
| `locals` | `locals.tf` | All local values |
| `terraform` | `terraform.tf` | Backend config, required providers, Terraform settings |

## Complete Project Example

A well-organized Terraform project following OTN:

```
project/
├── terraform.tf
├── locals.tf
├── provider.aws.tf
├── provider.aws.us-east-1.tf
├── variable.environment.tf
├── variable.region.tf
├── variable.project_name.tf
├── variable.instance_type.tf
├── variable.vpc_cidr.tf
├── variable.domain_name.tf
├── data.aws_ami.ubuntu.tf
├── data.aws_availability_zones.available.tf
├── data.aws_caller_identity.current.tf
├── resource.aws_vpc.main.tf
├── resource.aws_subnet.public.tf
├── resource.aws_subnet.private.tf
├── resource.aws_internet_gateway.main.tf
├── resource.aws_nat_gateway.main.tf
├── resource.aws_route_table.public.tf
├── resource.aws_route_table.private.tf
├── resource.aws_security_group.web.tf
├── resource.aws_security_group.db.tf
├── resource.aws_instance.web.tf
├── resource.aws_lb.web.tf
├── resource.aws_lb_target_group.web.tf
├── resource.aws_lb_listener.http.tf
├── resource.aws_rds_instance.primary.tf
├── resource.aws_route53_zone.primary.tf
├── resource.aws_route53_record.web.tf
├── module.monitoring.tf
├── output.vpc_id.tf
├── output.public_ip.tf
├── output.database_url.tf
└── output.load_balancer_dns.tf
```

## Best Practices

1. **Use descriptive block names** — choose clarity over brevity (`web-server` over `server`).
2. **Apply consistent naming across environments** — use the same block names with environment-specific variables.
3. **Start from day one** — apply OTN to new projects to avoid future refactoring.
4. **Run tfren before committing** — treat `tfren` as part of the standard pre-commit workflow alongside `terraform fmt` and `terraform validate`.
5. **Use tfren validate in CI/CD** — add `tfren validate --strict` to pull request checks to enforce OTN compliance automatically.
