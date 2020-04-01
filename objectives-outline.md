# Day 1

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
  - mention azure as the IaaS of choice
- what is a VM?
  - simulated machine with its own operating system running on a physical machine
  - can be rented from IaaS provider like azure
  - provisioned and run on demand
    - always on or as needed
  - why VMs?
    - powerful physical machines that can be subdivided
    - parity and portability
    - disposability and replication
    - allocate resources as needed
      - horizontal: replication
      - vertical: increasing allocation
      - scaling horizontal vs vertical (high level)
- how is a VM different than your local machine?
  - the VM is a simulation that a physical machine runs

## practical

- how do you connect to a hosted server?
  - local
  - W/LAN
  - internet
- publish and run
  - application locally
  - clone friend repo locally
  - clone repo remotely
- set up azure account / subscription
- basic in-memory application deployment
  - accessing the hosted application through browser / postman

## assumptions

- structure in place for students to have access to their own directories
  - subscription / credits taken care of
  - master account directory for instructor / TA to access the student directories
- capable of using bash (git bash / terminal) for basic CL navigation
  - git CLI
  - dotnet CLI
- all students using identical semver of dotnet SDK
- access to an in-memory version of an MVC or API application
- bash script for setting up the VM (through azure VM portal)


# Day 2

## conceptual

### Azure Key Vault

- What is sensitive data?
- What data should be in VCS?
- What data shouldn't be in VCS?
- What is a runtime environment?
- How can you inject variables into a runtime environment?
  - What is Configuration["SomeKey"]?
  - What is AppSettings.json?
- What is the Azure Key Vault?
- List three different ways to interface with the Azure Key Vault?
  - azure portal
  - dotnet
  - terminal (azure-cli)
- What are some examples of data you might keep in Azure Key Vault?
  - db config data
  - keys
  - cloud hosting config data

### Azure Active Directory B2C

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

### Azure Key Vault

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

### Azure Active Directory B2C

- How do you setup a dotnet application to use AADB2C to handle authentication?
- How do you access the identity of a user after they authenticate via AADB2C?
- How can you use the identity of a user to determine if they are authorized to access a resource?