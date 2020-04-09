=======
Hosting
=======

``Hosting`` is the act of deploy an application to a computer that is reachable by others. Throughout this class we have predominately hosted our applciations locally, the only person that could reach the application is the developer. We have been using Visual Studio to host our projects locally. Today we will learn about what is going on behind the scenes when we click the play button in Visual Studio.

Local Hosting
=============

Visual Studio was created to assist the developer, and one of the things it does for us is host our project locally with the click of a button. However, if you dig into what's happening behind the scenes we can start to learn about our first major concept in ``Operations``: building a project.

When you run your project Visual Studio is creating a version of your application that can be executed, and then running that excutable. This is all done behind the scenes so you can focus on development.

When we are ready to deploy our project our first step is to ``build`` our project.

``Building`` a project means to package project files together so that they are executable by any machine that has the .NET SDK.

Microsoft provides us with a very powerful tool for preparing our projects for publication: the ``dotnet cli``.

Using the ``dotnet cli`` we can publish our projects into a folder, and then we can send that folder to any other computer we want. As long as that computer has the .NET SDK it can run our project.

Today we are going to explore two ``dotnet cli`` commands: ``run`` and ``publish``.

Run
---

The first ``dotnet`` command we are interested in is ``dotnet run``. This command invokes the .NET SDK to run the selected .NET application.

When you click the green play button in Visual Studio it is simply running the ``dotnet run`` command behind the scenes. You can also manually run your project by navigating to your project directory in a terminal and entering ``dotnet run``.

Let's take a basic MVC .NET application and run it with the ``dotnet run`` CLI.

.. image:: /_static/images/azure-intro-hosting/dotnet-run.png

When we navigate to ``http://locahost:5000/`` we see:

.. image:: /_static/images/azure-intro-hosting/dotnet-run-view.png

Using ``dotnet run`` we have successfully run our .NET application, and it started our web app on the appropriate ports. This is essentially what Visual Studio is doing for us behind the scenes when we push the green play button.

Publish
-------

Being able to run our project is one thing, but right now our project is a combination of the code we have written, and all the libraries we have loaded as dependencies into our project. It would be great if we could package all of these together to make them easier to transport between computers. This is where the ``dotnet publish`` command comes in. This also bundles in all the requisite files so that the newly published project can be run with a lighter version of the .NET SDK. Any computer that has the runtime .NET SDK will be able to run our application.

Let's take a look at what happens when we run ``dotnet publish`` inside of the last application we ran.

.. image:: /_static/images/azure-intro-hosting/dotnet-publish-cli.png

It is telling us that it created a few new directories with our newly packaged project in ``bin/Debug/netcoreapp3.1/publish``.

When navigating and looking into this directory we can see a few things:

.. image:: /_static/images/azure-intro-hosting/dotnet-publish-cli-directory.png

And from here we can simply run our application with just the ``dotnet`` runtime using ``dotnet base-mvc.dll``.

.. image:: /_static/images/azure-intro-hosting/dotnet-base-mvc.png

Although these examples are still local they serve as a representation of how we will run our application on a server.

The steps to run a project on a server are:
    - publish a project
    - transfer published artifacts to remote server
    - download dotnet runtime SDK on remote server
    - run artifacts using the dotnet runtime SDK

Cloud Hosting
=============

In the previous section we learned how to host a .NET application on our local machine. This is a good exercise because it illustrates the steps necessary to deploy a project. However, there are a few issues with hosting an application on your local machine.

The first issue is that of networking. Our application is deployed on our machine, but it is only accessible on this machine, and other machines on our local network. You also must know the internal IP address of your machine in order to share your application with others on your network. We built our application for general users of the internet. Our local machine is connected to the internet, but it has no way of routing traffic on the internet to our local machine. We have a networking issue.

The second major issue if that of availability. Even if our local machine was available to general internet users they would only be able to access our application when our computer is on, and currently running our application. Ideally we will have some server that is always on, and always running our application so users can connect to our application at all times.

The final issue is that of discovery. Even if a general internet user could access our local machine, and our machine was always on and running our application a general internet user would have to know our exact IP address in order to access our application. Most Internet Service Providers assign IP addresses dynamically. So your IP address has the possiblity of changing. We need some permanent IP address, or name that a general internet user can use to access our application.

All three of these issues are solved by cloud hosting:
    - Networking
    - Discovery
    - Availability

Cloud Hosting allows us to overcome these issues by providing us with an always on server, that has a public IP address that can be associated with a domain name.

Cloud is a term used a lot in technology, but it's really just a collection of servers that are available on the internet, they usually contain additional services like data backup, or privacy in the form of authentication, or encryption, and more.

You can setup  your own servers and configure them to always be on, and to have public facing IP addresses however this can take a fair amount of setup. Most companies these days use Cloud Hosting Providers that do this configuration for them. So you no longer need to own a server to access a server. YOu simply rent one from a cloud services provider. Microsoft Azure is one of these Cloud Services Provider, and the tool we will be using throughout this class.

Using Azure we can easily, quickly, and cheaply start up our own server that has a public IP address, is always on, and can be configured with a DNS.

After creating a server, we will be able to copy over our Build Artifacts we created using ``dotnet publish``, and then run our application. It will then be accesible via the internet at a publicc IP address, and it will always be on and running. This allows our users to interface with our application. 