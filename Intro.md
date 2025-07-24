# Intro IaC

# Hands-on

### Setup

- Clone Repo
- Checkout new branch (tf_name)

## Add resource blocks

- Resource group

```terraform
resource "azurerm_resource_group" "rg_tfws" {
  location = "West Europe"
  name     = "rg-tf-workshop"
}
```

- Storage account

```terraform
resource "azurerm_storage_account" "st_tfws" {
  name                     = "stbtvdutfwsdev001"
  resource_group_name      = "rg-tf-workshop"
  location                 = "West Europe"
  account_tier             = "Standard"
  account_replication_type = "LRS"

  tags = {
    created_by = "Miri"
  }
}
```

- Role assignment

## Update resources

## Use variables

## Create & use modules

## Conditionals & Loops

## Import existing resources

- Storage account

- Role assignment

## Destroy resources