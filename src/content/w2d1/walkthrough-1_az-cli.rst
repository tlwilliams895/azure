:orphan:

.. _walkthrough-1_az-cli:

=====================================================
Provisioning & Managing Resources Using the Azure CLI
=====================================================

In this walkthrough we will practice using the ``az CLI`` to provision the same resources that we spun up using the web portal GUI. As you work through these commands try to picture the manual steps you performed on the web portal and use your knowledge of the ``az CLI`` patterns to translate those steps into CLI commands.

Although you can simply read through this guide it is important that you actively attempt each command on your machine. Reading and watching is passive learning but trying it yourself is the practical half of the learning process that helps solidify what you have learned.

Setup
=====

Before we begin we have to log in with our Azure subscription. This command will open your browser to enter your credentials. After logging in you can close the tab and return to your terminal.

.. sourcecode:: bash

    $ az login

Once you are logged in we will make use of the ``configure`` Command to set defaults for the CLI. This will make working with resources easier by automatically inserting default values for common required Arguments. Let's set a default Azure Location for our resources.

.. sourcecode:: bash

    $ az configure --default location=eastus

Now any resource that has a required Argument of ``--location`` or ``-l`` will automatically have ``eastus`` set as its value.

Resource Groups
===============

As always we will begin by provisioning a resource group to bundle all of the other resources associated with it. One of the benefits of this approach is that it is easy to dispose of all of the resources by deleting the resource group they are contained in instead of deleting each of them individually.

Exploration
-----------

Let's begin by using our trusty ``help`` option to learn about the available management commands for resource groups. As mentioned before resource groups are accessed under the Group name ``group``. 

.. sourcecode:: bash

    $ az group -h

Take a moment to look over the output. Because resource groups are simple containers there isn't much beyond the standard CRUD commands.

Tools & Planning
----------------

Of the available commands we can see that our goal is to ``create`` the resource. Let's dig into the ``create`` Command to see what Arguments are available.

.. sourcecode:: bash

    $ az group create -h

We will need to provide at minimum the two required Arguments:

- ``-n`` or ``-g``: the name of the resource group
- ``-l``: the Azure Location

Fortunately we have already configured our default Location value of ``eastus`` so all we need to supply is the resource group name. We will use a consistent naming convention to make sure none of our resources have conflicting names. The convention for resource groups we will use is "your name" + "wt" (walkthrough) + "rg" (resource group):

    <your name>-wt-rg

Provisioning
------------

Now that we have determined the command structure and its arguments we can create our resource group:

.. sourcecode:: bash

    $ az group create -n vamp-wt-rg

You should see a JSON output like this:

.. sourcecode:: bash

    {
        "id": "/subscriptions/<subscription ID>/resourceGroups/vamp-wt-rg",
        "location": "eastus",
        "managedBy": null,
        "name": "vamp-wt-rg",
        "properties": {
            "provisioningState": "Succeeded"
        },
        "tags": null,
        "type": "Microsoft.Resources/resourceGroups"
    }

Notice how the subscription and location are set automatically. The former by logging in and the latter by configuring its default value.

Configuring
-----------

Just as we set a default location we will assign this resource group as a default as well. Be sure to enter your new resource group name as the value:

.. sourcecode:: bash

    $ az configure --default group=vamp-wt-rg


Virtual Machines
================

Exploration
-----------

Tools & Planning
----------------

Provisioning
------------

Configuring
-----------

KeyVault Secrets
================

Exploration
-----------

Tools & Planning
----------------

Provisioning
------------

Configuring
-----------