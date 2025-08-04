# Intro IaC for Azure

https://spacelift.io/blog/terraform-resources

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

> Validate, commit & push your changes.  
> Run terraform plan & apply.

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

> Validate, commit & push your changes.  
> Run terraform plan & apply.

### Private endpoint

[Doku: azurerm/private_endpoint](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint)

```terraform
resource "azurerm_private_endpoint" "pe_datalake" {
  name                = "pe-xx-001"  # replace xx with name of storage account
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

- To create a private endpoint, we need to specify the `subnet_id`. We will use the existing subnet "xyz" in vnet "abc". How do we get its **id**?
- What is the `private_connection_resource_id` and how do we get it?
- Which `subresource_names` do we need to define for our datalake?

> Validate, commit & push your changes.  
> Run terraform plan & apply.

### Role assignment

```terraform
resource "azurerm_role_assignment" "st_sbdr" {
  scope                = ""
  role_definition_name = "Storage Blob Data Reader"
  principal_id         = "" 
}
```

- As scope we want to the define the storage account.
- Where/How do you find the `principal_id` of a user / group / service principal?

> Validate, commit & push your changes.  
> Run terraform plan & apply.

### Use variables

- Define a variable for **location** in variables.tf (type = string).
- Create a terraform.tfvars file and add `location = "West Europe"`.
- Replace all occurrences of the hard coded "West Europe" with the new variable.

> Validate, commit & push your changes.  
> Run terraform plan (_there should not be any changes_).

### Conditionals & Loops

#### Condition / Count



#### For each

- Create a local variable **datalake-private-endpoints** and assign the necessary subresource names:

  ```terraform
  locals {
    datalake-private-endpoints = {
      subresource_name = "001"
      ...
    }
  }
  ```

- Update the _first_ private endpoint resource:

  ```terraform
  resource "azurerm_private_endpoint" "pe_datalake" {
    for_each = local.datalake-private-endpoints

    name = "pe-xx-${each.value}"

    ...

    private_service_connection {
      subresource_names = [each.key]
      ...
    }
  }
  ```

- Delete the code for the _second_ resource.

> Validate, commit & push your changes.  
> Run terraform plan -> What will be changed?  

- Can we switch the datalake-private-endpoints definition from "id = name" to "name = id"? What would change?

> Run terraform apply.

## Create & use modules

- Create a module named "datalake"

  Terminal: 
  ```
  mkdir -p modules/datalake
  ```
  ```
  cd modules/datalake
  ```
  ```
  touch main.tf variables.tf output.tf
  ```

- Move the following resource definitions to the main.tf file of the module:
  - Storage account
  - Private Endpoint
  - Role assignment
- Create the necessary variable definitions in variables.tf.
- Update the resource definitions with the new variables.
- Add the following block to your main.tf file and add the variables.

  ```terraform
  module "test_datalake" {
  source = "modules/datalake"

  ...

  }
  ```

> Validate, commit & push your changes.  
> Run terraform plan -> What will be changed?  
> Run terraform apply.

### Dependencies between resources

- In which order are the resources created? 
- Are there any dependencies?
- If so, how can we make sure the resources are created in the correct order?


## Import existing resources

- Resource Group

- Role assignment

## Update TF files, if something was changed manually.

## Rename resources in state file (terrafrom state mv)

## Remove resources from state file (terraform state rm)