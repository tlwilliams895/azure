.. _intro_ws:

==============================
Introduction to Windows Server
==============================

Previously we used the Azure web portal to provision and deploy our API to an Ubuntu Linux VM. In order to execute commands and scripts to configure the deployment we used the RunCommand tool that was built into the web portal. 

Today we will learn about the other choice of OS for hosting our web applications -- Windows Server. Now that you are more comfortable working away from the web portal we will also learn how to remotely access our new Windows Server VM from the command line.

An Enterprise-Grade OS
======================

Windows Server 2019 (WS) is built on top of the same base as the consumer-grade Windows 10 operating system. While there are many Windows 10 editions for different use cases none of them are optimized for hosting web applications like WS.

Because WS is designed for computing it strips away many of the applications and features that are meant for consumers. The result is a smaller OS footprint that reduces resource usage. A smaller OS also means less code to consider for vulnerabilities. The reduced attack surface of the slimmer OS is complemented by sensible firewall defaults and other restrictions that further bolster its security.

Installation Options
--------------------

WS Desktop Experience
^^^^^^^^^^^^^^^^^^^^^

The default Windows Server VM image includes a full GUI shell just like Windows 10. The WS Desktop Experience edition makes working with WS as familiar as working on your machine at home. While it lacks consumer applications it does include a collection of administrative tools including the Windows Server Manager.

WS Core
^^^^^^^

Windows Server also comes in a **Core** edition which removes the desktop GUI entirely! Windows Server Core is even leaner than WS allowing you to start from the *core* and add just the features you need incrementally. You can read more about the WS Core design and comparison to the more opinionated WS Desktop Experience `in this article<https://docs.microsoft.com/en-us/windows-server/administration/server-core/what-is-server-core>`_. 

Server Manager
--------------

The Windows Server Manager is a powerful GUI-based tool for monitoring and managing your Windows Server. In more advanced contexts the Server Manager can even be used to manage a fleet of physical or virtual WS machines.

.. todo:: server manager screenshot

The Server Manager is the entry point of the WS VM and is presented to you after opening a Remote Desktop session. Besides being able to see high-level details about the machine you can also use it to configure the Roles and Features enabled on the server.

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

