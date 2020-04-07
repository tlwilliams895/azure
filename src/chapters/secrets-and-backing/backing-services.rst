================
Backing Services
================

A backing service is a service that provides data to an application. A database is an example of a backing system. A secret storage is an example of a backing service.

We are going to talk about a couple of different backing services offered by Azure most notably Azure SQL Services, and Key Vault.

External Application Dependencies
=================================

Our applications usually depend on various other pieces of technology. Our resources are stored in a database, usually relational, user access is logged and recorded to a logging service, project configuration can be loaded from external sources.

These additional services are known as external application dependencies. In other words our application will not behave properly without access to these underlying services. All of these services also need to be created, and maintained as a part of our greater tech stack.

Examples of External Application Dependencies include:
    - Database
    - Logging
    - Caching

As a final note for deployments typically each piece of your project will be on it's own infrastructure and will simply be networked together. Our VM is separate from our Key Vault, and both are separate from our Database service.

.. admonition:: note

   For simplicity sake we will be deploying our application and database to the same VM as networking is outside the scope of this class. But it is an excellent thing for you to look into on your own. How would you separate the services of your deployed project?