# assumptions

- structure in place for students to have access to their own directories
  - subscription / credits taken care of
  - master account directory for instructor / TA to access the student directories
- capable of using bash (git bash / terminal)
  - basic fs navigation
  - git
  - dotnet
- all students using identical semver of dotnet SDK
- access to an in-memory version of an MVC or API application
- bash script for setting up the VM (through azure VM portal)

!! major blocker !!

- students will have access to unix
  - w10+ subsystem
  - vm (too heavy)

# overview

- intro to cloud and
  - basic deployment of in-memory MVC
  - clone -> publish -> run -> connect
- more cloud, REST APIs
  - basic deployment of in-memory API
    - just CE resource (no users / members)
  - script: unit file and nginx
  - clone -> publish -> run -> connect
- secrets management and backing services
  - secrets management isolated example and deployment
  - configuring API
    - ef / mysql
    - secrets
    - still just CE resource (no users / members)
  - update API deployment
    - keyvault
    - script: to install mysql and setup the db
    - push -> clone -> publish -> run -> connect
- oauth
  - configuring API
    - clone with additional models
    - integrate adb2c (+ secrets)
- authorization
  - roles
  - documentation / swagger
  - final swagger endpoint for testing

# topics to include

- azure vm
  - day 1 onwards
  - dep: none
  - dev:
    - in-memory MVC
    - API progression
- REST / swagger
  - day 2
  - dep: none
    - how to run without users?
  - dev: in-memory API
    - just CodeEvent resource
    - no members or auth
  - ops: basic VM
- oauth / adb2c
  - dep: keyvault
    - complex, entire adb2c config
  - dev:
  - ops:
- secrets management (isolated sample and explain other use cases)
  - types
    - local: user-secrets
    - remote: keyvault
  - dep: none
  - dev: isolated example single endpoint API
    - /db -> returns connection string secret
      - with no secret: 404
      - with secret: "connected to: <connection string>"
    - returns value of a secret when requested
      - local: from user-secrets
      - remote: from keyvault
  - ops:
    - connection string API VM
    - keyvault
- backing service
  - ef / mysql
  - dep: keyvault
    - simple, a connection string
  - dev:
  - ops:
- roles
  - dep: adb2c
  - ops:
  - dev: backed API

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

- What is sensitive data?
- What data should be in VCS?
- What data shouldn't be in VCS?
- What is the difference between a config file and a secrets manager?
- Why do you use a secrets manager?
- Why should you keep your secrets manager separate from your config file?
- Why would you want to load external configurations into a project?
  - protect sensitive data
  - portability to multiple application environments
  - parity
    - what is an application environment
    - why do we have multiple application environments
      - difference between dev/testing/prod
    - only modify application environment config rather than internal code
- What is the difference between a configuration file, and a secrets manager?
  - config file -> portability between application environments
  - secrets manager -> handles and hides sensitive data
- What secrets manager do you use locally?
  - .NET user-secrets?
- What is AppSettings.json used for?
- What is the Azure Key Vault?
- List three different ways to interface with the Azure Key Vault?
  - azure portal
  - dotnet
  - terminal (azure-cli)
- What are some examples of data you might keep in Azure Key Vault?
  - db config data
  - keys
  - cloud hosting config data

## practical

- How do you add, list, and use secrets stored from a secrets manager?
- How can you add, list, and use configurations from an external configuration file?

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

# Day 4 -- OAuth

## conceptual

- What is authentication?
- What is authorization?
- What is identity?
- What is OAuth 2.0 implicit flow?
- What is a cookie?
- What is AADB2C?
- What process does AADB2C use to authenticate a user?
- Conceptually, How does AADB2C notify an application, or API of a user's identity?
- What is a claim in AADB2C?
- How do you access a claim in dotnet?

## practical


- How do you setup a dotnet application to use AADB2C to handle authentication?
- How do you access the identity of a user after they authenticate via AADB2C?
- How can you use the identity of a user to determine if they are authorized to access a resource?


# Day 5 -- Authorization & Swagger

## conceptual

## practical
