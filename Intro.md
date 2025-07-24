# Intro IaC for Azure

# Hands-on

### Setup

- Clone Repo
- Checkout new branch (tf_name)

## Create new resources

### Resource group

```terraform
resource "azurerm_resource_group" "rg_tfws" {
  location = "West Europe"
  name     = "rg-tf-workshop"
}
```

### Storage account - Datalake

[Doku: azurerm/storage_account](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_account)


```terraform
resource "azurerm_storage_account" "st_datalake" {
  name                           = ""
  resource_group_name            = "rg-tf-workshop"
  location                       = "West Europe"
  account_tier                   = "Standard"
  account_replication_type       = "LRS"
}
```

  - We want to create a datalake. What is the default value for `account_kind`, and do we need to change it?
  - What is the internal naming convention for storage accounts?
  - Do we need to define `public_network_acccess_enabled`? (try terraform plan & apply)


### Private endpoint

[Doku: azurerm/private_endpoint](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint)

```terraform
resource "azurerm_private_endpoint" "pe_datalake" {
  name                = "pe-stbtvdutfwsdev001-dev-001"
  location            = "West Europe"
  resource_group_name = "rg-tf-workshop"
  subnet_id           = ""

  private_service_connection {
    name                           = "example-privateserviceconnection"
    private_connection_resource_id = ""
    subresource_names              = []
    is_manual_connection           = false
  }
}
```

- To create a private endpoint, we need to define the `subnet_id`. We will use the existing subnet "xyz" in vnet "abc". How do we get its **id**?
- What is the `private_connection_resource_id` and how do we get it?
- Which `subresource_names` do we need to define for our datalake?


### Role assignment

```terraform
resource "azurerm_role_assignment" "st_sbdr" {
  scope                = azurerm_storage_account.st_datalake.id
  role_definition_name = "Storage Blob Data Reader"
  principal_id         = "" 
}
```

- Where/How do you find the `principal_id` of a user / group / service principal?

## Use variables

## Update resources

## Create & use modules

## Conditionals & Loops

## Import existing resources

- Storage account

- Role assignment

## Destroy resources