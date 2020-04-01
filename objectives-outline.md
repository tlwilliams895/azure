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
