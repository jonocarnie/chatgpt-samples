#Sample Congnitive Services sample



```terraform
provider "azurerm" {
  features {}
}

resource "azurerm_cognitive_services_account" "language" {
  name                = "my-language-service"
  location            = "westus2"
  resource_group_name = "my-resource-group"
  sku                 = "F0"
  kind                = "TextAnalytics"
  access_tier         = "Standard"
  
  identity {
    type = "SystemAssigned"
  }

  network_rules {
    default_action = "Deny"
    ip_rules       = []
    virtual_network_subnet_ids = []
  }

  encryption {
    customer_managed_key {
      key_vault_id = "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.KeyVault/vaults/<key_vault_name>"
      key_name     = "<key_name>"
      key_version  = "<key_version>"
    }
  }

  private_endpoint_connections {
    name                    = "my-priv-endpoint-conn"
    private_endpoint_id     = "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Network/privateEndpoints/<priv_endpoint_name>"
    private_link_service_id = "/subscriptions/<subscription_id>/providers/Microsoft.CognitiveServices/accounts/my-language-service"
    group_ids               = []
  }

  identity {
    type = "SystemAssigned"
  }

  network_acls {
    bypass                 = "AzureServices"
    default_action         = "Deny"
    ip_rules               = []
    virtual_network_subnet_ids = []
  }
}
```

Explanation:

1. The `provider` block specifies the Azure provider and its features that will be used to provision resources in Azure.

2. The `resource` block creates an Azure Cognitive Services account with the name `my-language-service` in the resource group `my-resource-group` located in the West US 2 region.

3. The `sku` parameter is set to `F0` and the `kind` parameter is set to `TextAnalytics` to create a Cognitive Language Service that is not free.

4. The `access_tier` parameter is set to `Standard` to enable customer-managed keys.

5. The `identity` block enables system-assigned managed identity for the Cognitive Language Service.

6. The `network_rules` block specifies the network rules for the Cognitive Language Service. The `default_action` parameter is set to `Deny` to block all incoming traffic from public networks. The `ip_rules` and `virtual_network_subnet_ids` parameters are set to `[]` to allow no exceptions.

7. The `encryption` block specifies the customer-managed key used for encryption. The `key_vault_id`, `key_name`, and `key_version` parameters are set to the respective values for the customer-managed key.

8. The `private_endpoint_connections` block specifies the private endpoint connection for the Cognitive Language Service. The `private_endpoint_id` parameter is set to the ID of the private endpoint. The `private_link_service_id` parameter is set to the ID of the Cognitive Language Service. The `group_ids` parameter is set to `[]` to allow no exceptions.

9. The `network_acls` block specifies the network ACLs for the Cognitive Language Service. The `bypass` parameter is set to `AzureServices` to allow traffic from Azure services. The `default_action` parameter is set to `Deny` to block all other traffic. The `ip_rules` and `virtual_network_subnet_ids` parameters are set to `[]` to allow no exceptions.