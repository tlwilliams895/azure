:orphan:

.. _walkthrough-1_az-cli:

==========================================
Provisioning Resources Using the Azure CLI
==========================================

In this walkthrough we will practice using the ``az CLI`` to provision the same resources that we spun up using the web portal GUI. As you work through these commands try to picture the manual steps you performed on the web portal and use your knowledge of the ``az CLI`` patterns to translate those steps into CLI commands.

Although you can simply read through this guide it is important that you actively attempt each command on your machine. The more practice you get the more comfortable you will get with using it.

For each resource we provision we will use a methodical approach:

- **Exploration**: use the help command to learn about the top level Group of the resource
- **Planning**: consider our tasks and what Commands, Sub-Groups and Arguments are needed to accomplish them
- **Provisioning**: put our Planning into action and provision the resources
- **Configuring**: any extra steps that use the new resource to complete our tasks

As you continue to work with the ``az CLI`` you will find this approach beneficial to efficiently navigating and putting together the commands you need. A good idea is to keep a running set of notes for easy access to common tasks you are faced with. These notes will prove invaluable when you begin composing commands together into scripts. 

Setup
=====

Before we begin you have to log in with your Azure subscription. This command will open your browser to enter your credentials. After logging in you can close the tab and return to your terminal.

.. sourcecode:: powershell

    > az login

Once you are logged in we will make use of the global ``configure`` Command to set defaults for the CLI. This will make working with resources easier by automatically inserting default values for common required Arguments. Let's set a default Azure Location for our resources.

.. sourcecode:: powershell

    > az configure --default location=eastus

Now any resource that has a required Argument of ``--location`` or ``-l`` will automatically have ``eastus`` set as its value.

Resource Groups
===============

As always we will begin by provisioning a resource group to bundle all of the other resources associated with it. One of the benefits of this approach is that it is easy to dispose of all of the resources by deleting the resource group they are contained in instead of deleting each of them individually.

Exploration
-----------

Let's begin by using our trusty help command to learn what management is available for resource groups. Although it's confusing remember that resource groups are accessed under the Group name ``group``.  

.. sourcecode:: powershell

    > az group -h

Take a moment to look over the output. Because resource groups are simple containers there isn't much beyond the standard CRUD commands.

Planning
--------

Of the available commands we can see that our goal is to ``create`` the resource. Let's dig into the ``create`` Command to see what Arguments are available.

.. sourcecode:: powershell

    > az group create -h

We will need to provide at minimum the two **required** arguments:

- ``-n`` or ``-g``: the name of the resource group
- ``-l``: the Azure Location

Fortunately we have already configured our default Location value of ``eastus`` so all we need to supply is the resource group name. We will use a consistent naming convention to make sure none of our resources have conflicting names. The convention for resource groups we will use is "your name" + "wt" (walkthrough) + "rg" (resource group):

    <first name>-wt-rg

Note that there is a 23 character limit for resource names. If you have a long first name consider using a short name like Pat if your name is Patrick.

Provisioning
------------

Now that we have determined the command structure and its arguments we can create our resource group:

.. sourcecode:: powershell

    > az group create -n <name>-wt-rg

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

.. sourcecode:: powershell

    > az configure --default group=<name>-wt-rg

You can confirm the default has been set by checking the CLI configuration with the ``-l`` (list) argument and seeing that the "group" has value has been set correctly:

.. sourcecode:: powershell

    > az configure -l

Virtual Machines
================

For this walkthrough we will not be using our VM to deploy an application but simply to get comfortable using the CLI. 

Exploration
-----------

Virtual Machines are naturally more complex to interact with than a simple resource group. However, now that we understand the pattern of the ``az CLI`` that complexity can be managed by taking our time to methodically work our way through it.

Once again let's begin by assessing what is available to us using the ``help`` argument:

.. sourcecode:: powershell

    > az vm -h

This time we see an abundance of Sub-Groups and Commands.

Planning
--------

Creating a VM will naturally require many Arguments to customize it. Recall in the web portal how there were several menus we had to work through. In addition to all of those options, CLIs expose additional configuration Arguments that serve more niche use cases. 

Let's see what Arguments are associated with creating a VM:

.. sourcecode:: powershell

    > az vm create -h

From this long list of arguments we will need to provide values for the following:

- ``-n``: the name of the VM
- ``--image``: the URN of the image used to create the VM
- ``--size``: the size of the VM
- ``--admin-username``: the username of the root account for the VM
- ``--assign-identity``: to assign an identity to the VM for accessing the KeyVault secrets

Listing Images
^^^^^^^^^^^^^^

In order to define the image for the VM we have to find its URN. In the ``vm create`` help output we saw a note that guided is in discovering these URN values. Let's list the available images using the ``vm`` Sub-Group ``image`` and its associated ``list`` Command:

.. sourcecode:: powershell

    > az vm image list

Many different images are provided in the JSON object list output. But all we care for is the URN values. We could manually scroll through all of them to find the URN of the Ubuntu image. Or we can make use of the global ``--query`` argument to output only the data we need!

The JMESPath query value we will use is ``"[].urn"`` which means take the list ``[]`` and instead of the full image objects only output the value of the ``urn`` property for each of them. The result is a list of just URN values which is much easier to work with!

.. sourcecode:: powershell

    > az vm image list --query "[].urn"

From here we can see the URN we need for the Ubuntu image is ``"Canonical:UbuntuServer:18.04-LTS:latest"``. Let's assign that value to a variable so we don't have to clutter our clipboard:

.. sourcecode:: powershell
    :caption: on Windows/PowerShell

    > $ImageURN="Canonical:UbuntuServer:18.04-LTS:latest"

.. sourcecode:: bash
    :caption: on Linux/BASH

    $ image_urn="Canonical:UbuntuServer:18.04-LTS:latest"

Now we can reference the URN by its variable name ``$ImageURN`` or ``image_urn`` depending on your shell.

.. tip::

    You can make use of a slightly more advanced query and in-line evaluation to do this in one step. Here we use a filter on the list to only output objects whose URN value contains the string Ubuntu. Then we pipe the result and access the first element's URN value.

    .. sourcecode:: powershell
    
        > az vm image list --query "[? contains(urn, 'Ubuntu')] | [0].urn"

    When we issue this command using in-line evaluation we can assign the resulting URN value output directly to the variable:

    .. sourcecode:: powershell
    
        > $ImageURN="$(az vm image list --query "[? contains(urn, 'Ubuntu')] | [0].urn")" 

    .. sourcecode:: bash
        :caption: If you are on Linux/BASH use the command below
    
        # -o: tsv sets the output to TSV format to remove the double quotes around the output
        # the quotes are treated as character literals by BASH and can break commands and scripts
        $ image_urn="$(az vm image list --query "[? contains(urn, 'Ubuntu')] | [0].urn" -o tsv)" 

Provisioning
------------

Now that we have our image URN we can provision the VM. We will use the following values for the remaining arguments:

- ``-n``: <name>-linux-vm
- ``--size``: Standard_B2s
- ``--admin-username``: student

.. note::

    It is important that you use these exact values so that it is easier to help you if something goes wrong along the way.

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

Next let's use the VM ``show`` Command to view all of the details of our new VM. The ``show`` Command requires the following Arguments:

- ``-n``: VM name (``--ids`` can be used in place of the name)
- ``-g``: the resource group the VM is in
- ``--subscription``: the subscription the VM is a part of

Since we have configured default values for each of these arguments we do not need to provide any of them to issue the command:

.. sourcecode:: bash

    $ az vm show

If you configured the default VM correctly you should receive a lengthy output object representing the state and configuration of the new VM. We will make use of the ``show`` Command when granting access to the KeyVault in the following section.

KeyVault Secrets
================

As our final step we will provision and configure our KeyVault.

Exploration
-----------

First explore the command using the ``keyvault`` Group name:

.. sourcecode:: powershell

    > az keyvault -h

From the KeyVault help we will need to use the ``secret`` Sub-Group along with the ``create`` and ``set-policy`` Commands.

Planning
--------

Looking back on the steps we performed in the web portal we will need to:

- create a KeyVault
- add a secret for the database connection string
- grant permission to the VM so it can access the connection string secret

Creating a KeyVault
^^^^^^^^^^^^^^^^^^^

To create a KeyVault we need to know what arguments it requires. Let's use the help command:

.. sourcecode:: powershell

    > az keyvault create -h

From the list of arguments we will need to provide:

- ``-n``: the name of the KeyVault
- ``-g``: the resource group name [default configured]
- ``-l``: the location [default configured]

Adding a Connection String Secret
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Let's see what command and arguments we need for creating the connection string secret:

.. sourcecode:: powershell

    > az keyvault secret -h

We can see that the ``set`` command is used to create or update a secret. What arguments does it require?

.. sourcecode:: powershell

    > az keyvault secret set -h

We will need to provide:

- ``-n``: the name of the secret
- ``--value``: the value of the secret
- ``--vault-name``: the name of the KeyVault the secret belongs to

Granting VM Access to the KeyVault
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After we provision the KeyVault we will need to set its access policy to allow the VM to read the connection string secret. Let's see what arguments the ``set-policy`` command takes:

.. sourcecode:: powershell

    > az keyvault set-policy -h

We will need to provide:

- ``-n``: the name of the KeyVault
- ``-g``: the resource group it belongs to [default configured]
- ``--object-id``: the VM object ID to uniquely identify it
- ``--secret-permissions``: space-separated list of access permissions to secrets for the VM

Of the many available permissions which should we choose to grant? The right choice is ``get list`` which equates to read-only access. We should grant the **least-privileged access** possible to the VM by only allowing it to read the secret names and values for this specific KeyVault. We do not want to expose the ability for the VM to modify, or worse, delete the secrets of our KeyVault if it were to fall into an attacker's hands. 

Provisioning
------------

First let's create the KeyVault itself. KeyVaults, unlike most other resources, have names that **must be globally unique across all Azure accounts**. For this reason we will need to use a unique pattern: 

    ``lc-<YY>-<name>-kv`` with ``YY`` standing for the current 2-digit year. 
    
This pattern should be unique but if you share a name with another student in the class just append your favorite number to the end and make note of it if requesting help from your instructor.

Before issuing the command let's store the KeyVault name in a variable since we will be using it more than once throughout our remaining tasks:

.. sourcecode:: powershell
    :caption: Windows/PowerShell

    > $KeyVaultName="lc-20-<name>-kv"
    > az keyvault create -n "$KeyVaultName"

.. sourcecode:: bash
    :caption: Linux/BASH

    $ keyvault_name="lc-20-<name>-kv"
    $ az keyvault create -n "$keyvault_name"

After the KeyVault has been provisioned let's set the connection string secret name and value:

- ``name``: "ConnectionStrings--Default"
- ``value``: "server=localhost;port=3306;database=coding_events;user=coding_events;password=launchcode"

.. tip::

    Recall that secrets are just entries of ``application.properties`` that we need to keep private and out of version control. The ``--`` is used as shorthand to define properties of JSON objects in a single "flat" string for the CLI command. In this case it is used to define a property called ``Default`` of a ``ConnectionStrings`` JSON object:

    .. sourcecode:: json

        "ConnectionStrings": {
            "Default": "<connection string value>"
        }

.. sourcecode:: powershell
    :caption: Windows/PowerShell

    > az keyvault secret set --vault-name "$KeyVaultName" -n "ConnectionStrings--Default" --value "server=localhost;port=3306;database=coding_events;user=coding_events;password=launchcode"

.. sourcecode:: bash
    :caption: Linux/BASH

    $ az keyvault secret set --vault-name "$keyvault_name" -n "ConnectionStrings--Default" --value "server=localhost;port=3306;database=coding_events;user=coding_events;password=launchcode"

Configuring
-----------

Now that the KeyVault and connection string secret have been set we just need to set the access policy for the VM.