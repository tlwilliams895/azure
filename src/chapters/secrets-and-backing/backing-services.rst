================
Backing Services
================

A backing service is a service that provides data to an application. A database is an example of a backing system. A secret storage is an example of a backing service.

We are going to talk about a couple of different backing services offered by Azure.

External Application Dependencies
=================================

Our applications usually depend on various other pieces of technology. Our resources are stored in a database, usually relational, user access is logged and recorded to a logging service, project configuration can be loaded from external sources.

These additional services are known as external application dependencies. In other words our application will not behave properly without access to these underlying services. All of these services also need to be created, and maintained as a part of our greater tech stack.

- Database
- Logging
- Caching
- Ideally live on separate infrastructure