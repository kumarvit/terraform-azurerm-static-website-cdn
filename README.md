# Azure Static website with CDN Endpoint

[![Terraform](https://img.shields.io/badge/Terraform%20-0.12-brightgreen.svg?style=flat)](https://github.com/hashicorp/terraform/releases) [![License](https://img.shields.io/badge/License%20-MIT-brightgreen.svg?style=flat)](https://github.com/kumarvna/cloudascode/blob/master/LICENSE)

Terraform Module to create an Azure storage account and enable the static website and create optional CDN service for the static website.

You can configure your storage account to accept requests from secure connections only by setting the `enable_https_traffic_only = true` for the storage account. By default, this property is enabled when you create a storage account using this module.

To defines the kind of account, set the argument to `account_kind = "StorageV2"`. Account kind defaults to `StorageV2`. If you want to change this value to other storage accounts kind, then this module automatically computes the appropriate values for `account_tier`, `account_replication_type`. The valid options are `BlobStorage`, `BlockBlobStorage`, `FileStorage`, `Storage` and `StorageV2`.

Note: *static_website can only be created when the account_kind is set to StorageV2.*

## These types of resources are supported

* [Storage Account](https://www.terraform.io/docs/providers/azurerm/r/storage_account.html)

* [Static Website](https://www.terraform.io/docs/providers/azurerm/r/storage_account.html#static_website)

* [Content Delivery Network (CDN)](https://www.terraform.io/docs/providers/azurerm/r/cdn_endpoint.html)

## Module Usage

Following example to create a storage account and set up a static website with CDN endpoint.

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

## Create resource group

By default, this module will not create a resource group and the name of an existing resource group to be given in an argument `resource_group_name`. If you want to create a new resource group, set the argument `resource_group_name = true`.

*If you are using an existing resource group, then this module uses the same resource group location to create all resources in this module.*

## Static Website

Azure Storage can serve static content (HTML, CSS, JavaScript, and image files) directly from a storage container named $web.  To learn more, see [Static website hosting in Azure Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website).

Set the argument `enable_static_website = true` and set the folder path for static content to
  `static_website_source_folder = "../artifacts/website"`.

## CDN Endpoint for Static Website

To add content delivery network acceleration to you static website set the argument `enable_cdn_profile = true` and set the  `cdn_profile_name` and `cdn_sku_profile` as an variables.

## `sensitive` — Suppressing Values in CLI Output

An output can be marked as containing sensitive material using the optional `sensitive = true` argument in the output declration.

Setting an output value in the root module as sensitive prevents Terraform from showing its value in the list of outputs at the end of `terraform apply`. It might still be shown in the CLI output for other reasons, like if the value is referenced in an expression for a resource argument.

Sensitive output values are still recorded in the [state](https://www.terraform.io/docs/state/index.html), and so will be visible to anyone who is able to access the state data. Storing state remotely can provide better security. For more information, see [Sensitive Data in State.](https://www.terraform.io/docs/state/sensitive-data.html)

## Inputs

Name | Description | Type | Default
---- | ----------- | ---- | -------
`create_resource_group` | Whether to create resource group and use it for all networking resources | string | `"false"`
`resource_group_name`|The name of an existing resource group.|string|`"rg-demo-westeurope-01"`
`location`|The location for all resources while creating a new resource group.|string|`"westeurope"`
`account_kind`|General-purpose v2 accounts: Basic storage account type for blobs, files, queues, and tables.|string|`"StorageV2"`
`enable_static_website` | Whether to create static website | string | `"false"`
`static_website_source_folder`|source folder path to copy files to static website storage blob|string|`""`
`enable_cdn_profile`|CDN profile and endpoint for static website|string|`"false"`
`cdn_sku_profile`|The pricing related information of current CDN profile.|string|`"Standard_Akamai"`
`Tags`|A map of tags to add to all resources|map|`{}`

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

## Authors

Module is maintained by [Kumaraswamy Vithanala](mailto:kumaraswamy.vithanala@tieto.com) with the help from other awesome contributors.

## Other resources

* [Azure Storage documentation](https://docs.microsoft.com/en-us/azure/storage/)

* [Terraform AzureRM Provider Documentation](https://www.terraform.io/docs/providers/azurerm/index.html)
