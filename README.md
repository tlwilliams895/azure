# Introduction to Microsoft Azure

## day 1: intro to hosting, azure & VMs

### setup

- linux subsystem (ubuntu 18.04 LTS)
  - CLI
    - dotnet
    - azure

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
  - set up azure accounts
  - how to provision and configure a publicly accessible VM
  - how to use the VM RunCommand console
    - configure dependencies
    - run the API
  - connect to a hosted project

## day 2: IaaS, web APIs & REST

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
  - use RunCommand
    - remove starter API
    - run CodingEvents API
  - connect to hosted swagger UI

## day 3: secrets management & backing services

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
    - use RunCommand
      - install and configure MySQL backing service
      - update and run latest API version

## day 4: OAuth, OIDC, azure ADB2C

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

### practical

- dev: explore ADB2C integration in the API
  - configuring appsettings.json for ADB2C
  - how to use the swagger UI to make authenticated requests
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
    - use RunCommand
      - install and configure nginx RP and openSSL cert
      - update and run latest API version
      - connect to hosted swagger UI

## day 5: authorization, next steps

### conceptual

- authorization
  - roles (identity associations, endpoint access, attribute access)
- next steps
  - decoupling services (managed databases, subnets)
  - load balancing
  - security (firewalls, vulnerabilities)
  - CI/CD (automation, code quality, testing)
  - application environments (dev, test, staging, prod)
  - monitoring (logging, notifications, automation)

### practical

- dev: exploring roles and authorization
- ops: deploy final completed API
