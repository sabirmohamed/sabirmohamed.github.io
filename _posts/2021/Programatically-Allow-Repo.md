---
title: Limit job authorization scope to referenced Azure DevOps repositories
date: 2021-01-26
categories:
  - DevOps
tags: [Azure DevOps, Pipelines, Automation]
classes: wide
---
{%- include toc -%}

# Intorduction

As you know pipelines often rely on multiple repositories that contain source, modules, templates, tools, scripts, or any other items that you need to build your code. By using multiple checkout steps in your pipeline, you can fetch and check out other repositories in addition to the one you use to store your YAML pipeline.

When you are checking out multiple repositories in your pipeline for intance calling a template from a diffrent repo you may encountered this permission warning and you will notice your pipeline has been queued until you manually intervine and click **permit**

Limit job authorization scope to referenced Azure DevOps repositories

**

**This pipeline needs permission to access a resource before this run can continue to Deploy**

And next step would Select View And Accept interactivley so that Pipeline will execute. 

### The Problem

Pipelines can access any Azure DevOps repositories in authorized projects unless this option is enabled.

