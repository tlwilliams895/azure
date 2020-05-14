:orphan:

.. _lesson-1_az-cli:

=============================
Introduction to the Azure CLI
=============================

Up to this point we have been using the Azure Web Portal to provision and manage our Azure resources. Using the terminology we have learned we would call this a graphical user interface, or GUI. GUIs are convenient and intuitive to use but inherently fall behind the capabilities of their text-based counterpart -- the CLI.

Why Should We Use the CLI?
==========================

The Azure Web Portal, with its intuitive layout and interactive menus, is actually just the "skin" that is backed by a comprehensive "brain" of a REST API. Any actions you take on the web portal to manage your Azure resources are ultimately fulfilled by HTTP requests sent `to this REST API <https://docs.microsoft.com/en-us/rest/api/azure/>`_. Can you think why they would develop a REST API that is distinct from the online GUI?

Recall that one of the many benefits of having a standalone API is that it operates **independent of any user interface(s) that consume it**. By separating their data management [API] from their presentation [UI] they are able to support flexibility and **multiple user interfaces** against a single consistent service. In the case of Azure that single REST API supports both the GUI web portal *and* the ``az CLI``.

.. todo:: diagram showing Azure -> REST API ->/-> Web Portal / CLI

At this point you understand the design decision of decoupling their API from their UIs, but the question still remains -- why should we bother using a CLI? Text-based interfaces are so *dated* aren't they? While GUIs are great to look at, and arguably offer a more intuitive experience *to a human*, they fall short in three key areas -- end user access, workflow speed and automation capability.

The GUI Delay
-------------

A GUI inherently requires additional development work to produce the layouts, buttons, and other components needed for a user to interact with it. Their graphical nature will always be more complex to develop and maintain than a text-based, CLI, counterpart.

This difference seems arbitrary to you as the end user. It may appear to only burden the Azure maintainers. But as the end user you are restricted to only using what has been developed by their teams. Whenever a new resource or service is supported internally by Azure it takes time for both GUI and CLI to have the feature added. The process looks something like:

#. internal development of new feature
#. implement support in the Azure REST API
#. implement support in the CLI
#. implement support in the GUI

If a hot new Azure feature is released, which will take more time to become available for you to use, the GUI or CLI? Because of the relative complexity GUIs always take more time to develop than making an addition to the CLI. Which means you can expect access to the new feature to be released to the CLI before, if ever, the web interface is updated.

And what about the more granular management tasks that you may need to perform on your resources? It stands to reason that priority in GUI development will always be given to the *most common* needs before reaching more granular or niche areas. Again, because of their simplicity, CLIs will have more more comprehensive features added to it that may never even reach the GUI. The only interface that exposes greater access to Azure resources over the CLI is the REST API itself.

.. admonition:: Fun Fact

    You can make requests directly to the REST API through any HTTP client like a browser, ``curl``, or the ``az CLI`` itself!

There are two key takeaways here about a system, of which Azure is but one example, which supports both a GUI and CLI:

- **the CLI will always receive the latest additions and updates before the GUI**
- **the CLI will always have more granular management capabilities than its GUI counterpart**

While the purpose of this lesson is to inspire your understanding and appreciation for CLIs they *are not always the best choice*. Sometimes GUIs offer an abstraction over more tedious work. Like most things in the development world you should not blindly adhere to a single approach. What is most important is selecting the tool that enables you to be more productive with your time. 

Work Velocity
-------------

When it comes to humans interacting with computers there is little doubt that GUIs are more pleasant to work with. But you are no mere human -- you are a technical powerhouse that has work to do! As a technical user your top priority is in choosing an interface that improves your workflow efficiency.

On the whole CLIs are the speedier choice for power users. They trade ease of exploration, beneficial to newcomers, for brevity. Whereas GUIs excel in visually guiding you, CLI tools leave it to you to be the guide. CLIs require you to be direct and precise with what you need done and by doing so allow you to work more efficiently. 

As an example, let's consider the process of provisioning a new VM from both the web GUI and the ``az CLI``.

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

On the one hand the GUI is helpful in providing visual cues and menus to guide you through the process. On the other hand the CLI allows you to skip directly to issuing exacty instructions for the work you need done. This example highlights the conciseness with regard to *manual* steps. But the real power of CLI tools comes in their automation capabilities.

Automation Showdown
-------------------

While the CLI is faster to work with we only looked through the lens of manually interacting with the two interfaces. Eventually the goal of any ops specialist is to automate their work! Automation is as much about saving valuable work time as it is about **ensuring consistent behavior**. 

.. tip::

    Computers excel at performing tasks exactly the same way every time. Whatever they are commanded to do they will do without fail or fatigue. Humans on the other hand are prone to introducing errors. For large complex systems the less human interaction involved the less likely that errors will occur. For this reason automation is a core tenant of modern development.

Let's revisit the example from earlier. But this time consider the task of provisioning 1000 VMs. Any human-based solution would require repeating steps 4-6 from above 1000 times. You can imagine that at some point the human would grow tired and as a result make a mistake in one or more of the configuration options. While humans don't have a "loop" ability our scripting languages certainly do!

Here is a basic example in PowerShell invoking the ``az CLI``:

.. sourcecode:: powershell
    :caption: powershell example

    for($VmCount=0; $VmCount -lt 1000; ++$VmCount) {
        az vm create <configuration options>
    }


Some of you might say, "Couldn't we write a browser script to automate navigating the web portal?" While this is possible it is significantly more complex than a 2-line loop. Worse yet is that GUIs, especially web-based ones, are more prone to updates and redesigns than CLIs. Which means if updates occur your script will likely break!

This is just one of thousands of automation examples you will come across in your career. We will explore semi-automatic and fully-automatic automation approaches in the coming sections. For now you can take away an appreciation for the CLI, as foreign as it may seem initially, as it will soon become one of your closest allies. 

Azure CLI Fundamentals
======================

The Azure CLI has been developed as an `open source <https://github.com/Azure/azure-cli>`_ cross-platform tool. Like the ``dotnet CLI`` it works the same whether you are executing it on Windows, Linux, or OSX through either of the supported shells, ``bash`` or ``PowerShell``. While it does not have the graphical help menus of the web GUI it does have a well organized pattern that makes "headless" navigation more intuitive than you may expect.

Just as with the GUI web portal the CLI will translate your command into an HTTP request sent to the Azure REST API. By default the responses received will be displayed as formatted JSON but you can request other formats if needed.  

The Pattern
-----------

The CLI is broken down into 3 main areas:

#. **Groups**: top-level services and resources
#. **Sub-Groups**: services, properties or features related to their parent **Group**
#. **Commands**: commands for managing a **Group** or **Sub-Group**

The general form of any command you enter will look like this:

.. sourcecode:: bash

    # <required argument>
    # [optional argument] 
    $ az <group> [sub-group] <command> [command options]

That's a bit abstract so let's break down the example we saw earlier:

.. sourcecode:: bash

    # Group: vm
    # Sub-Group: none
    # Command: create
    $ az vm create <configuration options>

Notice that the pattern is rather intuitive reading from left to right as "noun", "action", "customization". Or using the official terminology: 

    ``az [the program]`` + 
    ``resource or service [Group/Sub-Group]`` + 
    ``action to take [Command]`` + 
    ``options to configure the action [Command options]``

One other consideration to keep in mind is the dependency order. At minimum a Group is needed to issue a command. In order to access a Sub-Group it naturally requires a parent Group to be defined before it. By extension Commands are dependent on a Group or Sub-Group in the same way that a verb has no meaning without a noun to apply to.

Getting Help
^^^^^^^^^^^^

Another core aspect of the pattern in the ``az CLI`` is the use of a global ``help`` option. Despite being a text-based interface it can be surprisingly informative and detailed. The ``help`` option is attached to the end of any Group, Sub-Group, Command or combination among them. The help output depends on the context of what help is being requested but will often include sample examples of common use cases which can serve as a guide.

To use the ``help`` option simply append ``--help`` or its shorthand ``-h`` to the end of any CLI command.

.. tip::

    You can think of the ``help`` option as saying "help me with whatever is listed to the left of the help option"

Let's see some generic examples of how the option is used:

.. sourcecode:: bash

    # --help or -h may be used interchangeably

    # help with the tool itself (list available Groups and global Commands/options)
    $ az --help

    # help on a Group
    $ az <group> --help

    # help on a Sub-Group
    $ az <group> <sub-group> --help

    # help on a Group Command
    $ az <group> <command> --help

    $ help on a Sub-Group Command
    $ az <group> <sub-group> <command> --help

Notice how in each of these examples the pattern remains consistent in use. This makes it easy to build your understanding of the tool by digging through the help outputs through each Group, Sub-Group and Command. 


As a concrete example let's consider how to request help about the ``vm`` Group:

.. sourcecode:: bash

    $ az vm --help
    # or shorthand
    $ az vm -h

While the CLI may feel foreign initially you can use the ``help`` option at any time to guide you. It is in your best interest to practice digging through the Groups, Sub-Groups and Commands using the ``help`` option to familiarize yourself. Eventually you will intuitively recall the commands and options you need but whenever you need a reminder the help command is just one option away!

Groups
^^^^^^

Groups are the main resources and services that Azure CLI exposes control over. Some examples we will be using include:

#. ``vm``: Virtual Machine management
#. ``keyvault``: KeyVault management
#. ``group``: Resource Group management

.. note:: 

    For the purpose of explaining the organizational pattern we use the terms **Group**, **Sub-Group** and **Commands** to mirror the terminology used in the CLI and its official documentation. In practice when we refer to ``az group`` we will always mean **resource group**.

You can see all of the Groups available in the ``az CLI`` by entering the following ``help`` option (more on that later). Try entering this command in your terminal:

.. todo:: will they already have the az CLI installed to issue this command?

.. sourcecode:: bash

    $ az --help

Sub-Groups
^^^^^^^^^^

Within each of these Groups will be Sub-Groups of features or properties related to the parent Group. For example under the Group ``vm`` you would find the related Sub-Group ``identity`` which refers to the VM identity configuration. Similarly under ``keyvault`` you would find the Sub-Group ``secret`` for managing KeyVault secrets.

You can use the ``help`` command on a specific Group to view the Sub-Groups related to it:

.. sourcecode:: bash
    :caption: general form

    $ az <group> --help

Try entering the following commands to see the Sub-Groups related to the ``vm`` and ``keyvault`` Groups:

.. sourcecode:: bash
    :caption: vm and keyvault examples

    $ az vm --help
    $ az keyvault --help

Commands
^^^^^^^^

Commands are declaratively named actions that you can take on a Group or Sub-Group. They will typically include CRUD commands along with others that fit the context. The common CRUD commands you will see include:

- **C** - ``create``: create the resource
- **R** - ``show``: view an individual resource object
- **R** - ``list``: view a list of resource objects
- **U** - ``set``: update a property of a resource
- **D** - ``delete``: delete the resource

Just as before you can view the Commands associated with a Group or Sub-Group by using the ``help`` option. Within the context of a Group or Sub-Group you may also see more specific commands. These additional commands represent actions that can be taken on the particular resource. In other cases the special commands are conveniences for quickly accessing common properties of a resource.

For example looking at the Commands related to the ``vm`` Group (by using the ``help`` option) you will see many additional commands. Here are a few examples from the rather lengthy list:

.. sourcecode:: bash
    :caption: trimmed output of the many VM related commands

    $ az vm --help

    # commands specific to interacting with a VM resource
    open-port              : Opens a VM to inbound traffic on specified ports.
    perform-maintenance    : The operation to perform maintenance on a virtual machine.

    # shorthand convenience commands
    list-ip-addresses      : List IP addresses associated with a VM.
    list-sizes             : List available sizes for VMs.

Notice how many of these aren't available at all in the GUI! As a reminder Sub-Groups will also have their own Commands list which can be accessed the same way using the ``help`` option.

Command Options
^^^^^^^^^^^^^^^

Like most CLI tools commands also accept a series of **command options** or "flags" as they are sometimes referred to as. Think of these as modifiers for a given command. They are used to give additional context or configure settings for performing a command a specific way. The ``help`` option is itself an example of one of these that happens to apply *globally* and not just for one Command.

Just as Commands can be context-dependent on the Group or Sub-Group they are called on so too are the related options. The ``help`` option can be used on a Command to see the options associated with it. When reviewing the list of options take note of which options are **required** and which are optional -- typically only a small handful are actually required.

For example to see the options associated with creating a VM you can issue:

.. sourcecode:: bash

    $ az vm create -h

Note that these options can be exhaustive especially compared to what is available on the web portal. Don't be overwhelmed by them. They are always grouped and organized for easily finding which are relevant to your use case. 

.. todo:: should this section contain concept checks?

.. Concept Checks
.. ==============

.. At this point you have the fundamentals down and are likely eager to get to using this fancy new tool! But before continuing take a few minutes to go over the following questions and ensure you have understood the material.

.. .. admonition:: Question

..     Why do GUIs lag behind in exposing new features relative to CLIs?

..     #. 

.. todo:: seems out of scope to cover this, maybe best to just throw in as an example in the walkthroughs?

.. Query Filtering
.. ---------------

.. As mentioned previously all commands issued from the ``az CLI`` are sent as requests to the Azure REST API with response bodies displayed as JSON output. These response bodies can range from simple objects to lists with dozens of complex objects of data. Working with large complex response bodies can be a tedious and time consuming process.

.. Fortunately the ``az CLI`` includes a global option called ``--query`` which lets you transform the response body and hone in on just the data you need. The syntax used to define the transformation is a simple query language for JSON called JMESPath. We will not explore this syntax in great depth as it is beyond the scope of our learning goals. However, `the JMESPath documentation <https://jmespath.org/>`_ is well organized and has input boxes you can use to practice. 

.. What we will cover are the fundamentals which we will routinely use in our interactions with the ``az CLI``. The first step to using the ``--query`` option is to determine the shape of the data you are working with, which will be dependent on the command you issue. Fortunately there are only two types to consider as all of the commands will either output a single JSON object or a list containing multiple objects. 

.. .. tip::

..     While you can look through the documentation to determine the output shape to expect you can typically know based on the Command itself. Commands like ``list`` and those that interact with multiple resources or properties will output a list (even if there is only one element in that list). However, Commands that interact with a single resource or property directly will naturally output a single object.
