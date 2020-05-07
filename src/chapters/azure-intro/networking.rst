Networking
==========

With the abbreviated timeline of this course we don't have the luxury to go into Networking as it's own topic. However, understanding some of the most basic elements of networking is crucial to working with Cloud Service Providers. After taking this course we would recommend you to continue your own education of networking to prepare yourselves for your job, however we will cover the bare essentials for you to work through this curriculum.

**Networking** is the process of connecting multiple computers together. Networking includes the physical hardware (cables, computers, routers, etc), and the internal software configuration (IP addresses, access control, visibility, etc). Networking refers to both private networks (home network, office network), and public networks (the internet, a university/organization intranet).

IP Addresses
------------

An **Internet Protocol Address** (IP address) is a unique identification of each comptuer/device connected to a network that follows a specific numerical pattern.

The pattern of an IP address always follows ``xxx.xxx.xxx.xxx`` where the 'x's can be replaced by integers between 0 and 255.

Examples of valid IP addresses: 

- ``192.168.0.1``
- ``127.0.0.1``
- ``255.255.255.255``
- ``47.120.14.1``

.. admonition:: note

   For the purposes of this class we will only work with IPv4 addresses. It should be noted that a new format for IP addresses (IPv6) is also used, but goes beyond the scope of this class.

Local Area Network
------------------

A **Local Area Network** (LAN) is a network that connects all the computers/devices in a relatively small geographic location. You probably have a LAN in your place of residence, it may connect your computer, your smart phone, your printer, and anyone else in your house together on one network. Using the LAN you can send a print job from your computer to your printer and it may respond by printing the sent documents. Using your LAN you can access a shared hard drive, or move files between different devices.

Examples of places that may have a LAN:

- office
- hotel
- convention center
- restaurant

.. admonition:: note

   A LAN may have access to the internet, but only if internet access has been configured into the LAN.

Private IP Addresses
^^^^^^^^^^^^^^^^^^^^

A **private IP address** is an IP Address that identifies a device on a LAN.

An example of a private IP Addresses in a LAN would be your house. Your router acts as the device that manages the network. Since it's a device it gets a private IP address that makes it acessible to all the other devices on the LAN. The router may be assigned the private IP address ``192.168.0.1``. Once you connect a computer to the router via an ethernet cable, or by connecting to the router's wireless access point via a wireless card, the router will assign a private IP address to the device it may assign it the private IP address ``192.168.0.2``.

Internet
--------

Public IP Addresses
^^^^^^^^^^^^^^^^^^^

Processes & Ports
-----------------
