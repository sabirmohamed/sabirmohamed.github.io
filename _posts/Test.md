-
title: "Azure Policy and Initiatives"
date: 2019-05-21
author: [ "Sabir Mohamed" ]
category: automation
comments: true
featured: true
hidden: false
published: true
tags: [ policy, initiative, compliance, governance ]
header:
  overlay_image: images/header/whiteboard.jpg
  teaser: images/teaser/blueprint.png
sidebar:
  nav: "policy"
excerpt: Governance starts with policy compliance. Work through these labs to make Azure Policy and Initiatives work for you.
---

## Introduction

If you don't have standards in IT, things can get out of control and there's no change in cloud. In fact, there's more of a need for governance than ever, if you don't have it in cloud it could cause allot of issues, excessive costs, issues supporting and a bad cloud experience to name a few. Azure Policy is essentially designed to help set those standards on Azure use (using policies), making sure it's used in the right way highlighting issues (using compliance) and help fix those issues (using remediation).

You can manually create policies (to check Azure resources are configured in a certain way) or you can use the growing number of pre-built policies.

## Governance

We have labs for ARM templates, for Terraform configurations and for Azure Policies and Policy Initiatives. All of these will come together in a set of upcoming labs around initial customer governance in line with the [Cloud Adoption Framework](https://aka.ms/caf), plus the deployment of default shared services, policy assignments and role based access control (RBAC) assignments.

These will be achieved using a variety of tools, including Azure Blueprints, Terraform, subscription level ARM templates and Azure DevOps Pieplines, and will be aligned with common definition and deployment stages and organisational structures that we see in both partners and end customers.

Make sure you subscribe to the Azure Citadel [Atom (RSS) feed](/feed.xml) to get notified of the new content as it becomes available.

## Pre-requisites

Set up your machine for these labs using the [automation prereqs](/prereqs) page.

## Assumptions

Read through the [Azure Policy](https://docs.microsoft.com/en-gb/azure/governance/policy/overview) documentation area for your foundational knowledge.

----------

## Labs

**Lab** | **Description**
1 | [Policy basics in the portal](basics) | Use a simple policy to stipulate the permitted regions
2 | [Creating Policies via CLI](cli) | Specify the allowed VM SKU sizes using the Azure CLI
3 | [Custom Policies and Aliases](custom) | Use the vscode policy extension to determine aliases and create a custom policy
4 | [DeployIfNotExists](deploy) | Using a policy initiative with the DeployIfNotExist effect for automatic agent deployment and remediation
5 | [Management Groups and Initiatives](mg) | Step up a level using Management Groups and assigning a custom Deny initiative
6 | [Tagging and Auditing](tagging) | Enable default resource tagging without compromising innovation using the append and audit effects