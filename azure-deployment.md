What is our project?

How does it run locally?
	what are the tools?
		Visual Studio Run --> what's a different way to run our project than visual studio IDE?
		dotnet watch run
		local database
		environment variables

What needs to happen to prepare our project for deployment?
	infrastructure
		we will need a publicly accessible server -- VM
		we will need a DB -- Azure SQL DB
		we will need Azure Active Directory (for auth)
		we will need Azure Key Vault

	migrate DB to Azure SQL DB
		what is a DB migration?
		how can we do this easily?

	get project to run with AAD

	get project to run with Azure Key Vault

	run project locally and make sure it still works
		note about this is where having tests would be benefical

	build project locally to preapre for delivery
		how do you build a project in C#
		what are build artifacts?
		
	deliver artifcats to VM
		how can we do this?
		SSH? SCP?
		other tools

	prepare artifact for deployment?
		what are the steps to make these build artifacts runnable?
		install dependencies
		new user
		make executable
		execute?


Day 1: Cloud Computing, what is Azure, what is VM
	spin up a VM
	SSH into VM
	SCP a small project with no dependencies
	go through the steps to deploy project
		(build, SCP, make executable, create user, execute)

Day 2: Cloud Services & Cloud Networking
	setup key vault -- put data in it
	deploy small project that accesses key vault
	setup AAD
	deploy small project that uses AAD

Day 3: Azure SQL
	differences between local DB, and cloud backed DB on Azure
	spin up Azure SQL
	migrate local DB into Azure SQL
	deploy app that uses key vault for env vars to connect an app to AZ SQL
	
Day 4: Code Events

Day 5: Code Events & Azure Wrap-up
