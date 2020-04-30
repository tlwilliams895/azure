# Azure Course Overview

## Week 1: Introduction to Ops With Microsoft Azure

> **topics**: command line fundamentals, REST API deployment on linux and managing azure services through the web GUI

> **note**: deployments on linux VM this week

### day 1: intro to command line with bash and powershell

> **conceptual goals**: understand the similarities and differences between GUI and CLI, fundamental aspects and uses of OS shells

> **practical goals**: (bash and powershell) navigate and manage the file system, install and manage programs, basic scripting

### day 2: intro to hosting, azure & VMs

> **conceptual goals**: introduction to hosting, basic networking, azure and VMs

> **practical goals**: practicing publishing, executing and connecting to remote (W/LAN) machines

### day 3: IaaS, web APIs & REST
> **conceptual goals**: understand purpose and differences between IaaS and on-prem, review web APIs, REST and swagger 

> **practical goals**: explore dotnet web API structure and deploy a basic sqlite backed API to azure linux VM 

### day 4: secrets management & backing services

> **conceptual goals**: learn about backing services, external configurations and secrets management 

> **practical goals**: learn how to use local user-secrets and remote keyvault, deploy an API backed by an embedded mysql container to an azure linux VM

### day 5: OAuth, OIDC, azure ADB2C

> **conceptual goals**: learn about OAuth, OIDC, JWTs and azure ADB2C service

> **practical goals**: setup ADB2C tenant, deploy ADB2C API version with nginx reverse proxy for self-signed TLS termination

# Week 2: Command Line Ops And DevOps Troubleshooting With Azure

> **topics**: managing and automating azure deployment to a windows server VM from the command line, automated scripting, dev and ops troubleshooting, conceptual overview of out-of-scope ops topics

> **note**: deployments on windows server VM this week

### day 1: introduction to azure CLI and powershell scripting

> **conceptual goals**: learn about the az CLI and how to write automated powershell scripts and introduction to configurationa nd use cases for window services

> **practical goals**: how to configure and provision a windows server VM and keyvault with the az CLI, deploy the mysql backed API manually via CLI to windows server VM, develop an automating powershell script, how to configure and provision ADB2C with the az CLI

### day 2: Deploying Final Project to Windows Server, and Ubuntu Server via automated scripts using az CLI from both powershell and bash

> **conceptual goals**: continued exposure to powershell and bash scripting, continued exposure to the az CLI, introduction to SSH and SCP

> **practical goals**: continued practice provisioning and configuring Azure services (VM, KeyVault, ADB2C) via az CLI. contined practice of interfacing with Windows Server via RDP. Introduction of interfacing with Linux Servers via SSH, and SCP.

### day 3: Introduction to CI/CD with Azure Pipeline

> **conceptual goals**: Understand basic discrete steps of CI/CD, basic knowledge around Azure Pipeline, and ability to read various pipeline tasks

> **practical goals**: create, and configure an Azure Pipeline to automate various tasks of their Windows Server API deployment and to automate tasks of their Linux Server API deployment

### day 4: (devops troubleshooting) identifying, isolating, communicating and potential solutions

> **conceptual goals**: gaining independence with application-level troubleshooting and root cause analysis. gaining independence with ops-level troubleshooting and root cause analysis from the VPC to the VM

> **practical goals**: understanding timeouts and 5XX status codes, VPC-level issues, VM-level issues, communicating and implementing solutions. hands-on troubleshooting by accessing and analyzing logs, tracing 4XX/500 status codes, communicating and implementing solutions 

### day 5: next steps

> **conceptual goals**: closing exposure to other ops terminology, services, and best practices
