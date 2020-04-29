# Azure Static website with CDN Endpoint

Configuration in this directory creates Azure storage account and enable the static website and create optional CDN service for the static website. Few of these resources added/excluded as per your requirement.

## Configure the Azure Provider

Add AzureRM provider to start with the module configuration. Whilst the `version` attribute is optional, we recommend, not to pinning to a given version of the Provider.

## Create resource group

By default, this module will not create a resource group and the name of an existing resource group to be given in an argument `resource_group_name`. If you want to create a new resource group, set the argument `resource_group_name = true`.

*If you are using an existing resource group, then this module uses the same resource group location to create all resources in this module.*

## Adding TAG's to your Azure resources

Use tags to organize your Azure resources and management hierarchy. You apply tags to your Azure resources, resource groups, and subscriptions to logically organize them into a taxonomy. Each tag consists of a name and a value pair. For example, you can apply the name "Environment" and the value "Production" to all the resources in production. This is expected and must provide the following details as per your environment. There are no default values available for these arguments.

Here I have added the values directly. However, you can manage these as variables under `variables.tf` as well.  

```
  application_name      = "demoapp01"
  owner_email           = "user@example.com"
  business_unit         = "publiccloud"
  costcenter_id         = "5847596"
  environment           = "development"
```

## Module Usage

## Static Website

Following example to create a storage account with static website.

```
module "storageacc" {
  source                  = "github.com/tietoevry-cloud-infra/terraform-azurerm-static-website-cdn?ref=v1.1.0"
  create_resource_group   = false
  resource_group_name     = "rg-azure-westeurope-01"
  location                = "westeurope"
  storage_account_name    = "storageaccwesteupore01"
  # Static Website options
  enable_static_website   = true
  static_website_source_folder = var.static_website_source_folder
  # Tags to map
  tags = {
    application_name      = "demoapp01"
    owner_email           = "user@example.com"
    business_unit         = "publiccloud"
    costcenter_id         = "5847596"
    environment           = "development"
  }
}
```

## Static Website with CDN endpoint

Following example to create a storage account, static website with CDN endpoint.

```
module "staticweb" {
  source                  = "github.com/tietoevry-cloud-infra/terraform-azurerm-static-website-cdn?ref=v1.1.0"
  create_resource_group   = false
  resource_group_name     = "rg-demo-westeurope-01"
  location                = "westeurope"
  storage_account_name    = "storageaccwesteupore01"
  # Static Website options
  enable_static_website   = true
  static_website_source_folder = var.static_website_source_folder
# CDN endpoint for satic website
  enable_cdn_profile      = true
  cdn_profile_name        = var.cdn_profile_name
  cdn_sku_profile         = var.cdn_sku_profile
  tags = {
    application_name      = "demoapp01"
    owner_email           = "user@example.com"
    business_unit         = "publiccloud"
    costcenter_id         = "5847596"
    environment           = "development"
  }
}
```

## Terraform Usage

To run this example you need to execute following Terraform commands

```
$ terraform init
$ terraform plan
$ terraform apply
```

Run `terraform destroy` when you don't need these resources.

## Outputs

Name | Description
---- | -----------
`resource_group_name` | The name of the resource group in which resources are created
`resource_group_id` | The id of the resource group in which resources are created
`resource_group_location`| The location of the resource group in which resources are created
`storage_account_id` | The ID of the storage account
`sorage_account_name`| The name of the storage account
`storage_primary_connection_string`|The primary connection string for the storage account
`storage_primary_access_key`|The primary access key for the storage account
`static_website_url`|Static web site URL from storage account
`static_website_cdn_profile_name`|Name of the CDN profile
`static_website_cdn_endpoint_url`| CDN URL for static website