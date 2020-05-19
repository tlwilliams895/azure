.. _ws:

==============================
Introduction to Windows Server
==============================

Previously we used the Azure web portal to deploy our API to an Ubuntu Linux VM. In order to interact with and configure the VM we used the RunCommand tool in the web portal to issue commands. Today we will be exploring another popular OS for hosting our application -- Windows Server. 

Windows and Linux have many fundamental differences between them. But they also have many similarities as a basis for hosting our API. Deciding on a host OS can be a lengthy endeavor that, like all technologies, ultimately comes down to matching the right tool for the job. Rather than siding with one or the other it's important to begin by considering the abstract requirements needed to host your application.

Let's consider the common core of requirements that we would need to satisfy regardless of which OS is chosen:

- **package manager**: install and configure the environmental dependencies of our application
- ** 
- **DotNet SDK**: the SDK is needed to ``publish`` our artifact and the runtime is needed to ``run`` it.
- **

An Enterprise-Grade OS
======================

Security
--------

Server Manager
--------------

Installation Variants
---------------------

Remote Management
=================

Azure CLI
---------

Run-Command
^^^^^^^^^^^

Remote Desktop Protocol (RDP)
-----------------------------

MSTSC
^^^^^

Windows Remote Management (WinRM)
---------------------------------

PS-Session
^^^^^^^^^^

Invoke-Command
^^^^^^^^^^^^^^

Next Step
=========