---
title: How to Create a Service Connection in the Azure DevOps
date: 2020-12-28
categories:
  - Azure DevOps
tags:
  - Azure DevOps
classes: wide
---

{%- include toc -%}

## Introduction

A **Service Connection** is required for Azure DevOps Continuous Build and Continuous Release Pipelines to talk to external and remote services and execute tasks.

And Azure DevOps Pipelines support [various](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml#common-service-connection-types) service connection types few  are Azure Resource Manager, Azure Service Bus, Github, Kubernetes, Bitbucket, Docker Registry, etc.

In this blog, we will explore how to create a Service Connection to talk to an Azure Environment which can be used by Azure Pipelines. 

## Prerequisites

Before you begin this guide you'll need the following:

- Azure Subscription
- Azure App Registration (Service Principal) **In This Guide you will see how to create one**
- Azure DevOps Project

## Create an Azure App Registration 

You may have some confusions What is a Service Principal, Why we have an App Registration and Enterprise Application, is it the same?

Check out this explanation by [John Savil](https://www.youtube.com/watch?v=WVNvoiA_ktw). This video will provide much more clarity on Azure AD App Registrations, Enterprise Apps, and Service Principals.

Let's look at how you can create an Azure App Registration to solve this

Login to Azure Portal, navigate to Azure Active Directory, and Select **App registrations**

![step-1-Azure-AD-App-Registration.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-1-Azure-AD-App-Registration.png)

Select **New Registration**

![step-2-Select-New-Registration.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-2-Select-New-Registration.png)

Enter a Name for the Service Principal (SPN)

Since this is a Demo Project and Only will be working on this Tenant we have Selected Single Tenant, Select **Accounts in this organizational directory only**

And Select **Register**

![step-3-Register-An-Application.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-3-Register-An-Application.png)

You will need to a create a Secret (Password) to Authenticate to this SPN.

Select **Certificates & secrets**

![step-4-CertificatesSecrets.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-4-CertificatesSecrets.png)

Select **New client secret** and add **Description** and **Add**

![step-5-New-Client-Secret.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-5-New-Client-Secret.png)

**Save the Secret**

![step-6-Copy-the-Secret.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-6-Copy-the-Secret.png)

Now you have created an App Registration and a Client Secret, now we need to Assign this SPN Access to the Subscription

Navigate to *Subscription* Blade in the portal

Add and Select **Contributor**, Select the SPN you created earlier and Save.

![step-6-Grant-Contributor-to-Subcription.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-6-Grant-Contributor-to-Subcription.png)

## Create a service connection

In Azure DevOps navigate to relavant project, open the **Service connections** page from the project settings page. And Select **Create a Service Connection**

![create-service-connection.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-1-how-to-create-service-connection.PNG)

Select **Azure Resource Manager**

![step-2-select-azure-resource-manager.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-2-select-azure-resource-manager.PNG)

Now you can see that there few options to Authenticate to Azure Resource Manager

- Service principal (Automatic).
  - This will create an Azure AD App Registration for you automatically and use it in the service connection.

- Service pricipal (manual)
  - As you went through the process above in this article you know how to create a Service principal manually.

- [Managed Identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
  >Is another type of service principal in your Azure AD Managed by Azure, There are two types of Managed Idenitities which is System Assigned and User Assigned. Check out the following links if you want to further learn on this [Azure Managed Identities Explained in 5 Minutes](https://www.youtube.com/watch?v=1EoiGnQq14Y) by Azure Monk
  [Managed Identity in Azure DevOps Service Connections](https://stefanstranger.github.io/2019/03/02/ManageIdentityInServiceConnections/) 

- [Publish Profile](https://docs.microsoft.com/en-us/visualstudio/deployment/tutorial-import-publish-settings-azure?view=vs-2019#create-the-publish-settings-file-in-azure-app-service)
  >The publish profile is a file used to publish your web app or web job, it includes a username and password, it uses the basic auth to deploy your web app, if you use service principal/managed identity, it uses Azure AD authentication.

And we will authenticate using **Service Principal (Manual)**

![step-3-select-service-principal-manual.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-3-select-service-principal-manual.PNG)

A Service Connection can be targeted towards a Management Group, Subscription, and Machine Learning Workspace.

We will create this towards a Subscription and Select **Subscription** provide the following input parameters

- Subscription Id
- Subscription Name
- Service Principal Id (App Registration Application (client) ID)
- Service principal key (App Registration Client Secret)
- Tenant Id
- Service Connection Name and Description (Optional)

After Providing the parameters Select **Verify** and it will validate the SPN and Secret you will see Verification Succeeded.

Check the box for **Grant access permission to all pipelines**

Next Select **Verify and save**

![step-4-Fill-input-parameters_Updated.PNG](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-4-Fill-input-parameters_Updated.PNG)

![step-41-grant-access-permission-to-all-pipelines.PNG](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-41-grant-access-permission-to-all-pipelines.PNG)

Validate the Service Connection again in the portal

![step-5-Validate-in-the-portal.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-5-Validate-in-the-portal.PNG)

## Conclusion

- You Created an Azure App Registration (Service Principal)
- Assigned Contributor Rights and Added the Service Principal to the Subcription
- Created a Service Connection to Azure Resource Manager Authenticating via the Service Principal
- Verified the Connection

Now you can deploy resources to Azure using this Service Connection via Azure Pipelines

Next guide let's look at how we can create the same process with Azure CLI using a Configuration File and eventually integrate this creation to Azure DevOps Pipelines and Assigning Pipeline Permissions. 

>Thank you for visiting my blog 👋