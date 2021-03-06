---
title: Create a Azure DevOps Service Connection with Azure Devops CLI using a Configuration file
date: 2021-01-26
categories:
  - Azure DevOps
tags: [Azure DevOps, Automation]
classes: wide
---
{%- include toc -%}

### Introduction

Lately, I encountered some challenges in Automating the creation of a Service Connection in Azure DevOps.

After rambling on Google, found out that a Service Connection can be created by engaging the following tools.

- Azure DevOps REST API and PowerShell
- Azure CLI using a JSON configuration file invoking PowerShell
- VSTeam PowerShell Module [Add-VSTeamServiceEndpoint](https://methodsandpractices.github.io/vsteam-docs/docs/modules/vsteam/commands/Add-VSTeamServiceEndpoint)

As per [George Verghese](https://www.youtube.com/watch?v=DiztcJOZvZo) that,

>Microsoft has learned from the customers that Azure CLI is better than DevOps Rest API 
as it handles the authentication module better, since the CLI is an easy abstraction model on top of the REST API, it is a little easier interacting with the CLI
over the REST API

>In Terms of Donavan Browns VSTeams, it is more of a preference module like people who require PowerShell commanding, then VSTeams would be a better choice,
and in terms of a proper CLI, it might a better choice to go with the Azure CLI.

So in this guide let's dive into the creation of the service connection using Azure CLI with DevOps Extension, calling the parameters with a JSON Configuration File.

when you complete the guide you will be able to do the creation of a Service Connection with Azure CLI

### Prerequisites

Before you begin this guide you'll need the following:

- Azure Subscription
- Azure App Registration [Service Principal] - App ID and Secret [here](https://sabirmohamed.com/azure%20devops/Service-Connection-Creation-Manual-SPN/#create-an-azure-app-registration)
- Azure DevOps Project with a PAT [Personal Access Token]. You can find how you can create one [here](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page&WT.mc_id=AZ-MVP-5003674#create-a-pat)
- Azure CLI installed and configured with Azure DevOps Extention. If you don't have Azure CLI installed, you can download it [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli)
- Azure DevOps Project Id. You can find [below](https://sabirmohamed.com/azure%20devops/Azure-DevOps-Service-Connection-Creation-with-Azure-Devops-CLI-via-Configuration-file/#how-to-retreive-the-azure-devops-project-id) how to retrieve the id.

#### How to Retreive the Azure DevOps Project Id

You can get a collection of project properties via [REST API](https://docs.microsoft.com/en-us/rest/api/azure/devops/core/projects/get%20project%20properties?view=azure-devops-rest-6.0)

>But without a Postman or creating a Http Request, you can just enter the following API url in the browser replacing your organization name 

https://dev.azure.com/sabirmohamed/_apis/projects?api-version=6.0

And you will get the following response

![ProjectId.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ProjectId.png)


### Step 1 - Define the Configiuration in a JSON File

To build the following configuration file, you will need

  - Service Principal Id (App Registration Application (client) ID)
  - Service Principal Secret (App Registration Client Secret)
  - Tenant Id
  - Subscription Id
  - Subscription Name
  - Azure DevOps Project Name
  - Azure DevOps Project Id
  
```json
{
  "administratorsGroup": null,
  "authorization": {
    "parameters": {
      "authenticationType": "spnKey",
      "serviceprincipalid": "ba6a1e3a-038e-4e91-af69-2db54c446871",
      "tenantid": "63bba9bf-5820-408f-b151-c5ce0079c08b",
      "serviceprincipalkey": "Secret"
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
  "description": "Description - CloudSkills Sub",
  "groupScopeId": null,
  "isReady": true,
  "isShared": false,
  "name": "Service Connection Name - CloudSkills Sub",
  "operationStatus": null,
  "owner": "Library",
  "readersGroup": null,
  "serviceEndpointProjectReferences": [
    {
      "description": "Description - CloudSkills Sub",
      "name": "Service Connection Name - CloudSkills Sub",
      "projectReference": {
        "id": "23437652-1617-4294-abf5-468604d85da4",
        "name": "CloudSkills"
      }
    }
  ],
  "type": "azurerm",
  "url": "https://management.azure.com/"
}
```
The above configuration file will be targeted towards a Subscription, you can find a similar configuration targeted towards a Management Group [here](https://github.com/sabirmohamed/azure-devops-automation/blob/main/Service-Endpoints/ServiceConnectionMG.json)

Now we have the Configuration File in Place. Let's use this file to create the Service Connection

## Step 2 - Connect to Azure DevOps Organization via Azure CLI

First will add the Azure DevOps Extention
```
az extension add --name azure-devops
```
Login using the Personal Access Token [PAT]

```
az devops login --organization https://dev.azure.com/sabirmohamed
```
And paste your Token and you are In

Then Configure the Default Organization and Project using the following command

```
az devops configure --defaults organization=https://dev.azure.com/sabirmohamed project="CloudSkills"
```

## Step 3 - Create the Service Connection and Update Permission

Create the Service Connecting pointing the Configuration file
```
az devops service-endpoint create --service-endpoint-configuration .\ServiceConnectionSubcription.json
```

>You will see the output as below in JSON format with a additional CreatedBy Block which includes details of the user who initiated the creation.

![ServiceConnectionCreation.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ServiceConnectionCreation.png)

Validate the Service Connection in the Portal

![ServiceConnectionValidation.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ServiceConnectionValidation.png)

Once you created and validated the Service Connection you will see that the 

Grant access permission to all pipelines Box is unchecked.

![ServiceConnectionValidationGrantAccessUnchecked.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ServiceConnectionValidationGrantAccessUnchecked.png)

In orded to authorize and update this permission we will have to use the following switch. This switch cannot be applied during the creation of the service connection. 

[--enable-for-all](https://docs.microsoft.com/en-us/cli/azure/ext/azure-devops/devops/service-endpoint?view=azure-cli-latest#ext_azure_devops_az_devops_service_endpoint_update-optional-parameters)

First I will run the following command to list the Service Connection Id of the Service Connection that we just created

```
az devops service-endpoint list --output table
```
Copy the **Id** of the Service Connection, format the command as follows and run.

![ServiceConnectionIDRetreival.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ServiceConnectionIDRetreival.png)

```
az devops service-endpoint update --id 6404623d-5e21-41d2-882d-01f3df53fc23 --enable-for-all
```
![ServiceConnectionValidationGrantAccessChecked.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ServiceConnectionUpdatePipelinePermission.png)

Validate whether the permission is granted to authorize all pipelines

![ServiceConnectionValidationGrantAccessChecked.png](/Images/AzureDevOps/ServiceConnection_AzureCLI/ServiceConnectionValidationGrantAccessChecked.png)

## Conclution

- You populated the Configuration File
- Logged in to Azure DevOps with the CLI Extention using your PAT 
- Created a Service Connection using the Configuration File
- Verified the Connection
- Granted permission to access all pipelines and Validated

Now you can deploy resources to Azure using this Service Connection via Azure Pipelines and using this file you can 

Next guide lets look at how we can Secure the Service Connection. 

>Thank you for visiting my blog 👋



