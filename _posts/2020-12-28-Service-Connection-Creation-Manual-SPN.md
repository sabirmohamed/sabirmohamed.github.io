---
title: How to Create a Service Connection in the Azure DevOps
date: 2020-12-28
categories:
  - Azure DevOps
tags:
  - DevOps
classes: wide
---

In order to deploy resources to an Azure Environment with Azure Pipelines, you will need a Service Connection in Place. Let's explore how you can create a service connection in Azure DevOps.

## Prerequisites

Before you begin this guide you'll need the following:

- Azure Subscription
- Service Principal **In This Guide you will see how to create one**
- Azure DevOps Project

## Create an Azure App Registration 

You may have some confusions What is a Service Principal, Why we have an App Registration and Enterprise Application, is it the same?

Check out this explanation by [John Savil](https://www.youtube.com/watch?v=WVNvoiA_ktw). Will get you much more clarity on Azure AD App Registrations, Enterprise Apps, and Service Principals.

Let's look at how you can create an Azure App Registration to solve this

Login to Azure Portal, navigate to Azure Active Directory, and Select **App registrations**

![step-1-Azure-AD-App-Registration.PNG](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-1-Azure-AD-App-Registration.PNG)

Select **New Registration**

![step-2-Select-New-Registration.PNG](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-2-Select-New-Registration.PNG)

Enter a Name for the SPN 

Since this is a Demo Project and Only will be working on this Tenant we have Selected Single Tenant, Select **Accounts in this organizational directory only**

And Click **Register**

![step-3-Register-An-Application.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-3-Register-An-Application.png)

Click **Certificates & secrets**

![step-4-CertificatesSecrets.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-4-CertificatesSecrets.png)

Click **New client secret** and add **Description** and **Add**

![step-5-New-Client-Secret.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-5-New-Client-Secret.png)

**Save the Secret**

![step-6-Copy-the-Secret.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-6-Copy-the-Secret.png)

Now you have created an App Registration and a Client Secret, now we need to Assign this SPN Access to the Subscription

Navigate to *Subscription* Blade in the portal

**Add* and Select **Contributor**, Select the SPN and Save.

![step-6-Grant-Contributor-to-Subcription.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-6-Grant-Contributor-to-Subcription.png)

## Create a service connection

In Azure DevOps, **open** the Service connections page from the project settings page. And **Select** Create a Service Connection

![create-service-connection.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-1-how-to-create-service-connection.PNG)

**Select** Azure Resource Manager

![step-2-select-azure-resource-manager.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-2-select-azure-resource-manager.PNG)

**Select** Service Principal (Manual)

![step-3-select-service-principal-manual.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-3-select-service-principal-manual.PNG)

As this Service Connection is targeted towards a Subscription, **Select** Subscription and **provide** the following input parameters

- Subscription Id
- Subscription Name
- Service Principal Id (App Registration Application (client) ID)
- Service principal key (App Registration Client Secret)
- Tenant Id
- Service Connection Name and Description (Optional)

![step-4-Fill-input-parameters.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-4-Fill-input-parameters.PNG)

Validate again in the portal

![step-5-Validate-in-the-portal.png](/Images/AzureDevOps/ServiceConnection_ManualCreation/step-5-Validate-in-the-portal.PNG)


There you go, simple as that. In this guide,

- You Created an Azure App Registration 
- Assigned Permission 
- Created a Service Connection to Azure Resource Manager using the Service Principal
- Verified the Connection

Next guide lets look at how we can create the same process with Configuration File with Azure CLI and Powershell and eventually integrate this to Azure DevOps Pipelines.

Thank you for rambling around my blog, hope it was helpful.