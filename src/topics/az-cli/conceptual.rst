:orphan:

.. _az-cli_conceptual:

=============================
Introduction to the Azure CLI
=============================

Up to this point we have been using the Azure Web Portal to provision and manage our Azure resources. Using the terminology we have learned we would call this a graphical user interface, or GUI. GUIs are convenient and intuitive to use but inherently fall behind the capabilities of their text-based counterpart -- the CLI tool.

Why Should We Use the CLI?
==========================

The Azure Web Portal, with its colorful layout and interactive menus, is actually backed by a comprehensive REST API. Any actions you take on the web portal to manage your Azure resources is ultimately fulfilled by HTTP requests sent to this REST API. Can you think why they would develop a REST API that is distinct from the online portal?

Recall that one of the many benefits of having a standalone API is that it becomes **independent of any user interface(s) that consume it**. By separating their data management [API] from their presentation [UI] they are able to support **multiple user interfaces** against a single consistent service. In the case of Azure that single REST API can support both the GUI web portal *and* the companion CLI tool.

At this point you understand their design decision of decoupling their API from their UI, but the question still remains -- why should we bother using a CLI? While GUIs are great to look at, and arguably offer a more intuitive experience *to a human*, they fall short in two main areas -- development time and automation. 

The GUI Delay
-------------

A GUI inherently requires additional development work to produce the views, buttons, and other components needed for a user to interact with it. Their graphical nature will always be more complex to develop and maintain than a text-based, CLI, counterpart.

This difference seems arbitrary to you as the end user as it appears to only burden the Azure maintainers. However, you actually are impacted by the nature of having to wait for either of the interfaces to be developed or updated before you can use them. 

If a hot new Azure feature is released, which will take more time to become available for you to use, the GUI or CLI? And what about the more granular management tasks that you may need to perform on your resources? It stands to reason that priority in GUI development will always be given to the *most common* needs before reaching more granular or niche areas.

There are two key takeaways here about a system like Azure which supports both a GUI and CLI:

- **the CLI will always receive the latest additions and updates before the GUI**
- **the CLI will always have more granular capabilities than its GUI counterpart**

Ultimately what matters to you is selecting the interface that enables you to be more productive with your time.

Work Velocity
-------------

When it comes to humans interacting with computers there is little doubt that GUIs are more pleasant to work with. But you are no mere human -- you are a technical powerhouse that has work to do! As a technical user your top priority in choosing an interface has to do with the speed of your work.

While in some cases a GUI is faster to work with CLIs are, on the whole, the speedier choice. As an example, let's consider the process of provisioning a new VM from both the GUI and CLI.

Using the web portal you would need to:

#. open your browser
#. navigate to the azure portal and log in
#. search for the VM resource
#. load the "create VM" wizard
#. work through several menus to configure the VM
#. confirm and provision

Using the CLI you would need to open your terminal and enter a single command:

.. sourcecode:: bash

    $ az vm create <configuration option(s)>

Keep in mind that at first using the CLI will seem foreign to you. But as we will soon see the Azure CLI has an intuitive pattern to how it is organized and used. But the real magic of the CLI comes in the form of its automation potential.

Automation Showdown
-------------------

Azure CLI Fundamentals
======================

The Pattern
-----------

Getting Help
------------

Output Filtering
----------------
