

# Week 1: Introduction to Ops With Microsoft Azure

> goal: command line basics, REST API deployment and essential azure services through the web GUI

## day 1: intro to command line with bash and powershell

### conceptual

> goal: understand the similarities and differences between GUI and CLI, fundamental aspects and uses of OS shells

- shell basics
  - GUI vs CLI 
    - similarities: just a change in interface
    - differences: superset of GUI access with higher access
  - terminal emulator (high level: program for interfacing with the shell)
  - scripting languages
    - interacting with the OS
    - REPL
  - file system
    - navigation and management
    - PATH
  - CLI tools
- CLI tools
  - command programs
  - options (configuring commands)
  - operators (combining / controlling commands)
- scripting
  - configuration of machines
  - automating tasks

- ? text editors ?
  - VIM
    - https://stackoverflow.com/questions/49414257/which-program-to-edit-a-file-in-powershell-and-just-the-powershell

### practical

> goals: navigate and manage the file system, install and manage programs, basic scripting

- fundamentals
  - file system
    - working directory: view and change
    - view and change: working directory
    - files and dirs: create, read, delete, copy and move
  - install CLI tools: dotnet, azure 
- scripts
  - create and use variables
  - executing scripts

- windows
  - install powershell core
  - fundamentals (see above)
- linux
  - setup linux subsystem (ubuntu 18.04 LTS)
  - fundamentals (see above)

- walkthrough (bash and powershell)
  - show students how to write and execute a simple script
- studio (bash and powershell)
  - give use case and list of possible tools to use
  - list requirements and have students piece together a solution script

## day 2: intro to hosting, azure & VMs

### conceptual

- intro to hosting
  - local (loopback), W/LAN, internet
  - addressing (machine IPs, server process ports)
  - connecting (URLs, domain IP resolution)
- intro to azure
  - high level (cloud computing, azure services used in the course)
- intro to VMs
  - high level (virtualization, provisioning on-demand, parity)

### practical

- dev: dotnet web API starter
  - publishing and executing
  - connecting (local, WAN, internet)
  - how a project is published and executed outside of an IDE
    - ? self-contained or runtime-dependent ?
- ops: provision a VM deploy the starter
  - set up azure accounts and MFA
  - how to provision and configure a publicly accessible VM
  - how to use the use azure cloud shell
    - configure dependencies
    - run the API
  - connect to a hosted project

## day 3: IaaS, web APIs & REST

### conceptual

- IaaS
  - infrastructure (physical/virtual servers, networks, storage)
  - on-site vs cloud (availability, expenses, management, security)
  - scaling (horizontal, vertical)
- web APIs
  - review essentials (resources, JSON, endpoints)
- REST
  - review essentials (resource organization, methods, status codes)
  - swagger (OpenAPI spec, importance of documentation)

### practical

- dev: explore a basic web API
  - code
    - single resource (CodingEvent) API
    - SQLLite db
  - dotnet core
    - how an API is configured
  - swagger
    - how endpoints are documented
    - how to navigate and use the swagger UI
- ops: deploy the API to the VM
  - use azure cloud shell
    - build and run CodingEvents API
  - connect to hosted swagger UI

## day 4: secrets management & backing services

### conceptual

- backing services
  - external application dependency (db, logging, caching)
- application environment configs
  - parity and portability (dev, test, prod)
  - external configuration (public, secret)
  - version control
    - committed (source, public config)
    - ignored (derived, sensitive config)
  - secrets management
    - dotnet tooling
      - local: user-secrets
      - remote: keyvault

### practical

- dev: configuring API
  - use MySQL
  - configuring appsettings.json
  - configuring Startup.cs for secrets access
- ops: storing and managing secrets access
  - local
    - configuring dotnet user-secrets
    - storing db credentials as a secret
  - remote
    - provisioning KeyVault
    - configuring VM-KeyVault permissions
    - storing db credentials
  - deployment: update deployment with MySQL backed API
    - use azure cloudshell
      - install and configure MySQL backing service
      - update and run latest API version

## day 5: OAuth, OIDC, azure ADB2C

### conceptual

- oauth
  - fundamentals (authentication, authorization, delegation)
  - oauth flows (security, use cases)
  - JWT (transport medium, format, authenticity signatures)
- oidc
  - fundamentals (oauth extension spec, id tokens)
  - implicit flow
- Azure ADB2C
  - fundamentals (tenant, providers, scopes, flows)
- authorization
  - external (ops) vs embedded (dev) 
  - RBAC vs ABAC
- IAC (Infrastructure as Code)

### practical

- dev: explore ADB2C integration in the API
  - configuring appsettings.json for ADB2C
  - how to use the swagger UI to make authenticated requests
  - exploring roles and authorization
- ops: ADB2C setup
  - provision and configure ADB2C tenant directory
    - connect ADB2C directory to primary subscription
  - configure
    - API application
      - redirect URIs
      - API access and published scopes
    - local identity provider
    - SUSI flow (provider and claims)
  - deployment: update deployment with ADB2C API
    - use azure cloud shell
      - install and configure nginx RP and openSSL cert
      - update and run latest API version
      - connect to hosted swagger UI

# Week 2: Command Line Ops And Troubleshooting With Azure

> goal: managing and automating Azure resources from the command line, dev and ops troubleshooting, conceptual overview of out-of-scope ops topics

- changes: greater empahsis on windows server and powershell

## day 1:

> goal: introductory powershell scripting and service management with az CLI

### conceptual

- exploring azure CLI (alternative to GUI portal)
- combining powershell scripting with az CLI
- RDP
- IIS

### practical

- configure az CLI
- access directory and services
- ops: deploy API from previous week
  - manual
    - CLI provisioning
      - keyvault
      - VM
      - ADB2C
    - RDP as entrypoint to remote server
      - deliver codebase via git
      - install and configure MySQL via powershell
      - Configure IIS and it's dependencies
        - Create a self-signed cert from IIS
  - scripting
    - develop a powershell script to automate the manual CLI steps

## day 2:

> goal: continued powershell scripting and service management with az CLI, introduction to az CLI for Linux Servers

### practical

- ops: deploy API to both Windows Server, and Ubuntu
  - manual
    - CLI provisioning
      - KeyVault
      - VM
      - ADB2C
    - CLI deployment of API
      - RDP for Windows Server via powershell
      - SSH & SCP for Ubuntu via bash
  - scripting
    - Powershell
    - Bash
  - set up nginx in Ubuntu

## day 3: Introduction to CI/CD with Azure Pipeline

> goal: understand basic steps of CI/CD and basic Azure Pipeline tasks

### conceptual
- CI/CD basics & Pipeline Tasks
  - VCS connection
  - Build
  - Deliver
  - Deploy(?)
  
### practical
- Azure Pipeline
  - Windows Server
    - VCS connection
    - Build
    - Deliver
    - Deploy
  - Ubuntu
    - VCS connection
    - Build
    - Deliver
    - Deploy(?)

## day 4: (devops) identifying, isolating, communicating and potential solutions

> goal: gaining independence with devops troubleshooting

### conceptual

- logging
  - VPC and VM
- root cause analysis (interpreting failure)
  - connection timeout
  - 5XX status codes
  - VPC
    - network security groups
    - RP / load balancer / gateway
  - VM
    - application security groups
    - firewalls
- communicating
  - phrasing and terminology
- logging
  - understanding exceptions and stack traces
- root cause analysis (interpreting failure)
  - 4XX/500 status codes
  - narrowing the scope of your questions
    - alternating question terms
  - trusting sources
- communicating
  - phrasing and terminology

### practical
- logging
  - viewing logs from az CLI
- analysis
  - az CLI debugging
- reproducing
  - prove isolation
- communicating
  - opening issues
- logging
  - viewing logs from az CLI
  - CRD logs to Blob Storage (using Powershell piping to analyze)
- analysis
  - google search operators
  - browser dev tools (network, console for CORS)
  - IDE debugging (application)
- reproducing (proving isolation)
  - codebases
    - API (familiar)
    - lc101 app (foreign)
- communicating
  - opening issues

## day 5: next steps

> goal: closing exposure to other ops terminology, services, and best practices

### conceptual

- next steps
  - decoupling services (managed databases, subnets)
  - load balancing
  - security (firewalls, vulnerabilities)
  - CI/CD (automation, code quality, testing)
  - application environments (dev, test, staging, prod)
  - monitoring (logging, notifications, automation)
  - docker containerization
  - PKI
  - 12 factor apps (cloud-native devops)