---
title: Azure DevOps Service Connection Creation with Azure CLI and Powershell
date: 2021-01-03
categories:
  - DevOps
tags:
  - DevOps
---

### Introduction

Lately I encountered some challanges on Automating the creation of a Servince Connection in Azure DevOps.

After rambling on Google, found out that the a Service Connection can be created enganging the following tools.

- Azure DevOps REST API and PowerShell
- Azure CLI using a JSON configuration file invoking PowerShell
- VSTeam PowerShell Module [Add-VSTeamServiceEndpoint]

According to Gwarkey a PM for Azure DevOps, Microsoft has learned that Azure CLI is better than DevOps Rest API 
as it handles the authentication module better, since the CLI is a easy absraction model on top of the REST API, it is little easier interacting with the CLI
over the REST API"

In Terms of Donavan Browns VSTeams, it is more of a preference module like people who require PowerShell need to commanding then VSTeams would be a better choice,
and in terms of a proper CLI, it might a better choice to go with the Azure CLI.

So this article lets dive in to the automation using Azure CLI with DevOps Extension, calling the parameters with a JSON Configuration File invoking PowerShell.

when you complete the guide you will be able to do the creation of a Service EndPoint with CLI

### Prerequisites

Before you begin this guide you'll need the following:

- Azure Subcription
- Azure App Registration [Service Pricipal] - App ID and Secret
- Azure DevOps Project with a PAT [Personal Access Token]. You can find how you can create one [here](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page&WT.mc_id=AZ-MVP-5003674#create-a-pat)
- Azure CLI installed and configured with Azure DevOps Extention. If you don't have Azure CLI installed, you can download it here.

### Step 1 - Define the Configiuration in a JSON File

This Configuration fill be targeted towards a Subcription, you can find one for Management Group [here](github URL)

```json
{
  "administratorsGroup": null,
  "authorization": {
    "parameters": {
      "authenticationType": "spnKey",
      "serviceprincipalid": "19464646-c6f2-44658-ac6462-d6466446c8c2c",
      "tenantid": "63646469bf-5820-44f-b151-c55464668b",
      "serviceprincipalkey": ".xRfm7.0oRuYw5-wCQex1hYLt08~oc.kw7"
    },
    "scheme": "ServicePrincipal"
  },
  "data": {
    "creationMode": "Manual",
    "environment": "AzureCloud",
    "scopeLevel": "Subscription",
    "subscriptionId": "06e6d20f-d8ff-4658-8441-2a687f0effea",
    "subscriptionName": "Visual Studio Enterprise Subscription"
  },
  "description": "Visual Studio Enterprise Subscription",
  "groupScopeId": null,
  "isReady": true,
  "isShared": false,
  "name": "Service Connection Sub for Demo",
  "operationStatus": null,
  "owner": "Library",
  "readersGroup": null,
  "serviceEndpointProjectReferences": [
    {
      "description": "This Azure DevOps Service Connection is for Validation",
      "name": "Service Connection Sub for Demo",
      "projectReference": {
        "id": "03f0b5d6-4a44-408a-abaa-6b8ae5842939",
        "name": "Sabir Dev"
      }
    }
  ],
  "type": "azurerm",
  "url": "https://management.azure.com/"
}
```

Now we have the Configuration File in Place. Lets use this to create the Service Endpoint

## Step 2 - Connect to Azure DevOps Organization via Azure CLI

First will add the Azure DevOps Extention
```
az extension add --name azure-devops
```
Login to using the PAT

```
az devops login --organization https://dev.azure.com/sabirmohamed
```
And paste your Token and you are In

Then Configure the Default Organization and Project using the following command

```
az devops configure --defaults organization=https://dev.azure.com/sabirmohamed project="DevOps Dojo"
```

## Step 2 - Create the Service Connection

Create the Service Connecting pointing the Configuration file
```
az devops service-endpoint create --service-endpoint-configuration .\ServiceConnectionSub.json
```
Validate 









