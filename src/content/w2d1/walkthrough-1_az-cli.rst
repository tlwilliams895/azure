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

We will need to provide at minimum the two **required** arguments:

- ``-n`` or ``-g``: the name of the resource group
- ``-l``: the Azure Location

Fortunately we have already configured our default Location value of ``eastus`` so all we need to supply is the resource group name. We will use a consistent naming convention to make sure none of our resources have conflicting names. The convention for resource groups we will use is "your name" + "wt" (walkthrough) + "rg" (resource group):

    <first name>-wt-rg

Note that there is a 23 character limit for resource names. If you have a long first name consider using a short name like Pat if your name is Patrick.

Provisioning
------------

Now that we have determined the command structure and its arguments we can create our resource group:

.. sourcecode:: bash

    $ az group create -n <name>-wt-rg

You should see a JSON output like this:

.. sourcecode:: bash

    {
        "id": "/subscriptions/<subscription ID>/resourceGroups/<name>-wt-rg",
        "location": "eastus",
        "managedBy": null,
        "name": "<name>-wt-rg",
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

    $ az configure --default group=<name>-wt-rg

You can confirm the default has been set by checking the CLI configuration with the ``-l`` (list) argument and seeing that the "group" has value has been set correctly:

.. sourcecode:: bash

    $ az configure -l

Virtual Machines
================

For this walkthrough we will not be using our VM to deploy an application but simply to get comfortable using the CLI. 

Exploration
-----------

Virtual Machines are naturally more complex to interact with than a simple resource group. However, now that we understand the pattern of the ``az CLI`` that complexity can be managed by taking our time to methodically work our way through it.

Once again let's begin by assessing what is available to us using the ``help`` argument:

.. sourcecode:: bash

    $ az vm -h

This time we see an abundance of Sub-Groups and Commands.

Tools & Planning
----------------

Creating a VM will naturally require many Arguments to customize it. Recall in the web portal how there were several menus we had to work through. In addition to all of those options, CLIs expose additional configuration Arguments that serve more niche use cases. 

Let's see what Arguments are associated with creating a VM:

.. sourcecode:: bash

    $ az vm create -h

From this long list of arguments we will need to provide values for the following:

- ``-n``: the name of the VM
- ``--image``: the URN of the image used to create the VM
- ``--size``: the size of the VM
- ``--admin-username``: the username of the root account for the VM
- ``--assign-identity``: to assign an identity to the VM for accessing the KeyVault secrets

Listing Images
^^^^^^^^^^^^^^

In order to define the image for the VM we have to find its URN. In the ``vm create`` help output we saw a note that guided is in discovering these URN values. Let's list the available images using the ``vm`` Sub-Group ``image`` and its associated ``list`` Command:

.. sourcecode:: bash

    $ az vm image list

Many different images are provided in the JSON object list output. But all we care for is the URN values. We could manually scroll through all of them to find the URN of the Ubuntu image. Or we can make use of the global ``--query`` argument to output only the data we need!

The JMESPath query value we will use is ``"[].urn"`` which means take the list ``[]`` and instead of the full image objects only output the value of the ``urn`` property for each of them. The result is a list of just URN values which is much easier to work with!

.. sourcecode:: bash

    $ az vm image list --query "[].urn"

From here we can see the URN we need for the Ubuntu image is ``"Canonical:UbuntuServer:18.04-LTS:latest"``. Let's assign that value to a variable so we don't have to clutter our clipboard:

.. sourcecode:: bash
    :caption: on Linux/BASH

    $ image_urn="Canonical:UbuntuServer:18.04-LTS:latest"

.. sourcecode:: powershell
    :caption: on Windows/PowerShell

    > $ImageURN="Canonical:UbuntuServer:18.04-LTS:latest"

Now we can reference the URN by its variable name!

.. todo:: too heavy IMO

.. .. tip::

..     You can make use of a slightly more advanced query and in-line evaluation to do this in one step. Here we use a filter on the list to only output objects whose URN value contains the string Ubuntu. Then we pipe the result and access the first element's URN value.

..     .. sourcecode:: bash
    
..         $ az vm image list --query "[? contains(urn, 'Ubuntu')] | [0].urn"

..     When we issue this command using in-line evaluation we can assign the resulting URN value output directly to the variable:

..     .. sourcecode:: bash
    
..         $ image_urn="$(az vm image list --query "[? contains(urn, 'Ubuntu')] | [0].urn")" 

Provisioning
------------

Now that we have our image URN we can provision the VM. We will use the following values for the remaining arguments:

- ``-n``: <name>-linux-vm
- ``--size``: Standard_B2s
- ``--admin-username``: student

.. note::

    It is important that you use these exact values so that it is easier to help you if something goes wrong along the way.

.. Before we create the VM let's consider how we will use the output data. In the next step we will create and configure our KeyVault. As part of the configuration we will need to grant access to the secrets using the VM's system identity. We could 

Let's create our VM! Note that this command will take some time to complete.

.. sourcecode:: powershell
    :caption: Windows/PowerShell

    > az vm create -n <name>-linux-vm --size "Standard_B2s" --image "$ImageURN" --admin-username "student" --assign-identity

.. sourcecode:: bash
    :caption: Linux/BASH

    $ az vm create -n <name>-linux-vm --size "Standard_B2s" --image "$image_urn" --admin-username "student" --assign-identity

You should receive an output like this:

.. sourcecode:: bash

    {
        "fqdns": "",
        "id": "/subscriptions/<subscription ID>/resourceGroups/<name>-wt-rg/providers/Microsoft.Compute/virtualMachines/<name>-linux-vm",
        "identity": {
            "systemAssignedIdentity": "37dac2fe-ad6b-4b35-b03c-082b6f6bc524",
            "userAssignedIdentities": {}
        },
        "location": "eastus",
        "macAddress": "00-0D-3A-18-98-5F",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "13.72.111.180",
        "resourceGroup": "<name>-wt-rg",
        "zones": ""
    }

Notice how the default resource group value you set earlier was automatically included along with the subscription and location.  

Configuring
-----------

Before we continue let's set this VM as the default:

.. sourcecode:: bash

    $ az configure --default vm=<name>-linux-vm

Next let's use the VM ``show`` Command to view all of the details of our new VM. If you configured it correctly you should receive a lengthy output object representing the state and configuration of the VM.

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