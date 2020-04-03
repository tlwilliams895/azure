# requested changes

- swagger from beginning

# overview

## day 1: intro to hosting, azure and VMs

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

## day 3: Secrets Management & Backing Services

## conceptual

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

## practical

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

# Setup

## programs

- CLI
  - dotnet
  - azure
- GUI
  - firefox
  - postman

## linux subsystem

- https://docs.microsoft.com/en-us/windows/wsl/install-win10
  - https://docs.microsoft.com/en-us/windows/wsl/initialize-distro
  - ubuntu 18.04

# Day 1 - Intro to Azure & VMs

## conceptual

- what does it mean to host an application?
  - running a server process on a machine
    - host: the machine running the server process
  - exposing an address (origin) for accessing the server
    - networking (high level)
      - local
      - W/LAN
      - internet
- what is an IP address and port?
  - IP: a machine's address
  - port: a server process on the machine's address
- what are the parts of a server's URL?
  - with a domain name
  - with an IP address
  - ports (explicit and implicit, 80 and 443)
- what is the relationship between a domain name and an IP address
  - localhost and 127.0.0.1
  - DNS (high level)
    - phone book analogy
    - do not get into resolution chaining
- what allows dotnet to run on any OS?

  - published executable artifacts that can be run on any OS that has the dotnet runtime
    - publishing: compiling and packaging
  - SDK is needed to write and publish
  - runtime is needed to execute
  - java :: c#
    - SDK
    - JVM :: CLR
    - JRE :: VES

- intro to azure
  - overview of services well be using
  - segue and focus on VM
- what is a VM?
  - simulated machine with its own operating system running on a physical machine
  - can be rented from IaaS provider like azure
  - provisioned and run on demand
    - always on or as needed
  - why VMs?
    - powerful physical machines that can be subdivided
    - parity and portability
- how is a VM different than your local machine?
  - the VM is a simulation that a physical machine runs

## practical

- How can you provision a VM from Azure?
- How can you run scripts on a VM from the Azure web GUI?

- walkthrough: how do you connect to a hosted server?
  - introduce basics of curl
  - local
    - run a an app server and use browser / postman to make requests
  - W/LAN
    - instructor gets WLAN network IP, runs app, and shares with class to show access on a local network
    - bonus: add entry to /etc/hosts to show domain resolution to the shared IP
  - internet
    - connect to portal.azure.com
      - open dev tools and show the IP address
- studio: publish -> run -> connect
  - clone starter repo
    - add an endpoint that says "you connected successfully!"
      - endpoint that is curled for subsequent steps
  - application locally
    - CLI: publish -> run
    - connect (local): curl
  - clone friend repo locally
    - CLI: clone -> publish -> run
  - connect (local): curl
  - clone repo remotely
    - set up azure account / subscription
    - set up VM
    - run commands in VM console
      - predefined
        - setup script (dependencies)
      - commands
        - clone
        - publish
        - run
        - curl
          - to reinforce concept of control over VM (just like local machine)
    - get public IP
      - connect (remote): curl
        - show connecting to a publicly hosted application

# Day 2 -- IaaS and REST

## conceptual

- what is cloud hosting (IaaS)
  - infrastructure
    - powerful server machines organized into self-sustaining data centers
    - provisioning virtual machine slices of physical server machines
    - networking services
      - internal
      - public
    - powerful persistence machines
  - service
    - allocating machines to host applications
    - managing and networking
  - why IaaS?
    - self hosting vs cloud hosting (capex + opex)
      - simplified: renting vs owning in terms of expenses and time management
      - high availability and scaling
        - responsibility shifted to the IaaS provider
          - provisioning resources
          - maintaining uptime
          - managing redundancy
  - VM
    - allocate resources as needed
    - disposability and replication
      - horizontal: replication
      - vertical: increasing allocation
      - scaling horizontal vs vertical (high level)
- what is a web API?
  - an interface for controlled access and management of data (resources) over the web
  - endpoints consisting of a path and a method
    - each has some association with an action to take on data
  - consumes and produces data (JSON)
- what is the difference between an MVC application and a web API?
  - MVC returns HTML the API returns data
  - an API is agnostic to its front end consumer(s)
  - pros/cons decoupling development and deployment of frontend and backend
- What is REST?
  - a specification for designing APIs with standardized navigation and behavior
    - a standard that API consumers can rely on for consistent behavior
    - represents access to resources by their endpoints
      - collections and entity
  - most common HTTP methods
    - association with CRUD actions on data
  - status code ranges and their meaning
    - no specifics in the XX

## practical

- studio: deploy API
  - basic deployment of in-memory API
    - just CE resource (no users / members)
  - script: unit file and nginx
    - nginx: TLS termination as a "black box"
      - a program handling certificates for us
      - do not go into RP or how it works
    - unit file: standardized way of running a server process in linux
      - do not go into detail of systemctl / service just syntax for usage
  - clone -> publish -> run -> connect

# Day 3 -- Secrets Management & Backing Services

## conceptual

- what is an application environment?
  - the environment the application runs in including external variables
  - why use environments instead of hard-coding?
    - parity and portability
      - why do we have multiple application environments
        - difference between dev/testing/prod
      - only modify application environment config rather than internal code
    - protect sensitive data
- What is sensitive data?
  - credentials and other non-public variables that the application uses
  - why store sensitive data separate from the application source code?
    - prevent publicizing the information
- What information should and shouldn't be in git?
  - include
    - source code
    - public configuration (appsettings.json)
  - exclude
    - derived code (libraries, published package)
    - sensitive data
- what is a secrets manager?
  - a tool that stores sensitive data outside of the source code
  - local and remote tools available for dotnet projects
    - stored in key:value entries
      - objects are accessed using : notation for nesting
    - local: user-secrets
    - remote: keyvault
      - What are some examples of data you might keep in Azure Key Vault?
        - db config data
        - keys
        - cloud hosting config data
  - Why do you use a secrets manager?
    - for sensitive external configuration
- What is the difference between a config file and a secrets manager?
  - config: public data
  - sm: private data
- what are backing services?
  - external services that the application depends on
  - examples
    - database
    - logging
    - caching

## practical

- how do you access external configuration settings in your application?
  - Startup -> Configuration.GetValue/Section or utility wrappers (GetConnectionString)
    - "dot" notation using : for nesting
- How do you add and list secrets stored in a secrets manager?
- How do you create a Key Vault?
  - Azure Web GUI
  - terminal (azure-cli)
- How do you add items to a Key Vault?
  - Azure Web GUI
  - terminal (azure-cli)
- How do you access (READ) items in a Key Vault?
  - dotnet framework
  - Azure Web GUI
  - terminal (azure-cli)
- How do you connect a Key Vault to a dotnet project?
  - whatever dependencies, and configs that need to be added to a project
- How can you add and view configurations in appsettings.json?

### studio

- local
  - !! must be in project directory !!
  - configure user-secrets for project
    - command: \$ dotnet user-secrets init
    - describe what the secrets ID is and show where it is saved to (CodeEventsAPI.csproj)
      - user secrets are defined per project
  - create user-secrets entry
    - command: \$ dotnet user-secrets set "ConnectionStrings:Default" "Server=localhost;Port=3306;Database=coding_events;User=coding_events;Password=password"
      - note: the connection string label "Default" is arbitrary but must match how it is consumed in the application
  - list user-secrets
    - command: \$ dotnet user-secrets list
- API
  - branch: 2-mysql-start
  - TODOs: assign the connection string from external configuration
- remote
  - azure portal (screenshots)
    - create key vault
    - add connection string
    - assign VM identity
    - grant VM identity access to key vault
  - run scripts in VM
    - docker -> mysql -> set environment for db setup
    - update code in VM
    - publish -> restart service -> connect

# Day 4 -- ADB2C

## conceptual

- visual oauth
  - What is authentication?
  - What is authorization?
  - what is delegation?
- What is AADB2C?
  - an oauth provider service
  - what is OIDC?
    - https://medium.com/@darutk/diagrams-of-all-the-openid-connect-flows-6968e3990660
    - https://christianlydemann.com/implicit-flow-vs-code-flow-with-pkce/
    - a wrapper around oauth that authorizes access to and packages a user identity
    - What is OAuth 2.0 implicit flow?
      - adb2c process diagram
    - What is an identity token?
      - a form of authentication
      - a collection of data uniquely identifying a user in JWT format
      - what is a JWT?
        - a JSON object that has been encoded and signed
        - signature: proof of authenticity
        - encoding: compresses and separates the context of the JWT
        - payload claims
          - key value pairs: identity, authorization, and metadata

## practical

### studio

- azure
  - how to configure an adb2c tenant
  - how to link the adb2c tenant directory with your primary directory and subscription
  - how to add and configure
    - adb2c application
      - redirect URIs
        - local
        - jwt.ms (for testing)
        - remote (added later but point out location)
      - api access / published scopes
    - local account identity provider
      - email vs username
    - SUSI flow
      - selecting a provider
      - requested claims: user attributes
        - custom claim (username)?
      - provided claims: application claims
  - how to get adb2c configuration data
    - TenantId
    - etc...
- API
  - add adb2c config to appsettings.json
  - explore how users are registered in the db
    - accessing claims
- local
  - publish and run
  - azure
    - adb2c directory
    - adb2c
    - user flows
    - run user flow
      - jwt.ms
      - copy token
    - postman
      - open a request
      - headers
        - key: Authorization
        - value: Bearer <copied token>
- remote
  - update code in VM
  - publish -> run
  - connection: repeat postman tests pointing at hosted API

# Day 5 -- [Next Steps] Authorization & Swagger

## conceptual

## practical
