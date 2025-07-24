# Intro IaC for Azure

# Hands-on

### Setup

- Clone Repo
- Checkout new branch (tf_name)

## Add resource blocks

### Resource group

```terraform
resource "azurerm_resource_group" "rg_tfws" {
  location = "West Europe"
  name     = "rg-tf-workshop"
}
```

### Storage account

[Doku: azurerm/storage_account](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_account)


```terraform
resource "azurerm_storage_account" "st_tfws" {
  name                           = "stbtvdutfwsdev001"
  resource_group_name            = "rg-tf-workshop"
  location                       = "West Europe"
  account_tier                   = "Standard"
  account_replication_type       = "LRS"
}
```

  - What is the default value for `account_kind`?
  - Do we need to define `public_network_acccess_enabled`? (try terraform plan & apply)


### Private endpoint



### Role assignment

```terraform
resource "azurerm_role_assignment" "st_sbdr" {
  scope                = ""
  role_definition_name = "Storage Blob Data Reader"
  principal_id         = "" 
}
```

  - Where do you find the `principal_id` of a user / group / service principal?
  - How do we set the `scope` to the storage account that was created before?

## Update resources

## Use variables

## Create & use modules

## Conditionals & Loops

## Import existing resources

- Storage account

- Role assignment

## Destroy resources