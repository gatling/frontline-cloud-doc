---
title: "Azure Load Generators"
description: "Load Generators on your private Azure account"
lead: "Private Locations on your Azure account"
date: 2023-03-31T15:29:00+00:00
lastmod: 2023-03-31T15:29:00+00:00
weight: 22053
---

## Azure Virtual Machines
Azure private locations require the control plane to have Azure credentials configured in order to instantiate virtual machines and associated resources.

### Environment variables
Credentials can be set through environment variables in your control plane.

| name                  | value             |
| --------------------- | ----------------- |
| AZURE_CLIENT_ID       | Client UUID       |
| AZURE_CLIENT_SECRET   | Client secret key |
| AZURE_TENANT_ID       | Tenant UUID       |
| AZURE_SUBSCRIPTION_ID | Subscription UUID |

Check Azure documentation pages to find these values:
* [Tenant id](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-to-find-tenant)
* [Client id and secret](https://learn.microsoft.com/en-us/answers/questions/834401/hi-i-want-my-client-id-and-client-secret-key)
* [Subscription id](https://learn.microsoft.com/en-us/azure/azure-portal/get-subscription-tenant-id)

### Control plane configuration file

```bash
control-plane {
  # Control plane token
  token = "cpt_example_c7oze5djp3u14a5xqjanh..."
  # Control plane token with an environment variable
  token = ${?CONTROL_PLANE_TOKEN}
  # Control plane token with a system property
  token = $?control.plane.token
  # Control plane description (optional)
  description = "my control plane description"
  # Locations configurations
  locations = [
    {
      # Private location ID, must be prefixed by prl_, only consist of numbers 0-9, 
      # lowercase letters a-z, and underscores, with a max length of 30 characters
      id = "prl_private_location_example"
      # Private location description (optional)
      description = "Private Location on Azure"
      # Private location type
      type = "azure"
      # Azure location name, as listed by Azure CLI:
      # az account list-locations -o table
      region = "westeurope"
      # Virtual machine size, as listed by Azure CLI:
      # az vm list-sizes --location "westeurope"
      size = "Standard_A1"
      # Certified AMI configuration
      image {
        type = "certified"
        java = "latest" # Possible values : 8, 11, 17 or latest
      }
      # Azure subscription id as returned by Azure CLI:
      # az account show
      subscription = "<MySubscription UUID>"
      # Full identifier of Azure Virtual Network to use for your load generators
      # Use "id" field as returned by Azure CLI:
      # az network vnet list
      network-id = "/subscriptions/<MySubscription UUID>/resourceGroups/<MyResourceGroup>/providers/Microsoft.Network/virtualNetworks/<MyVNet>"
      # Subnet belonging to previously defined virtual network
      # Use "subnets.name" as returned by Azure CLI:
      # az network vnet subnet list --resource-group MyResourceGroup --vnet-name MyVNet
      subnet-name = "default"
    }
  ]
}
```
