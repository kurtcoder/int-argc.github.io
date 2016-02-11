---
layout: post
title: Redis Tutorial
permalink: /redis-tutorial/
---

##Application Development Tutorial

###Redis Tutorial
Redis is often described as a key-value datastore, but what is a datastore? We can think of it like this: Redis is a database that offers data structures which can be used to store your data.

In this tutorial you will learn how to deploy a sample JSP application in Bluemix.  In addition, you will also learn how to create a PostgreSQL database service that will be used by the sample application.

>**Prerequisite:**

>Basic background in web application development is required in this tutorial.
>Basic knowledge in databases.
>Basic knowledge in data structures.

<br>

####Basic Knowledge
1. Redis offers data structures. Data structures are good...

1. Fill-up and submit the registration form.

1. Wait for a confirmation e-mail and follow the instructions in the e-mail to validate your Bluemix registration.

<br>

####Explore your Bluemix Account

1. Open a web browser and login to your [Bluemix](https://ibm.biz/bluemixph) account.

1. You will be redirected to your dashboard.  Your dashboard:
	- provides some information regarding your account (e.g., organization, spaces, etc.)
	- summarizes the amount of resources you have consumed 
	- enumerates the applications, services, containers, and virtual machines you have created.

	<br>

1. You may have one ore more organizations.  By default, you only have one organization.  The name of this organization is the same as your Bluemix account.

	Additional organizations may become available in your Bluemix account when other Bluemix users share his/her organization to you.  The procedure in sharing an organization is not covered in this tutorial.
	
	<br>
	
1. Each organization has an allotted set of resources.  As an example, your dashboard shows the following:


	
	
	Resource  | Consumed | Total Allocation 
	--- | --- | --- 
	Cloud Foundry Apps | 0GB | 2GB 
	Services and APIs | 0 | 10 

	Note that there are other resources in your dashboard not listed above.  In addition, the amount of total allocation may vary depending on the type of account (e.g., trial account, etc.)

	Having a total allocation of 2GB for Cloud Foundry apps means that you can have one or more running applications in your account that have a total memory consumption of 2GB.  As an example, if you deploy Application A in Bluemix with a memory allocation of 1GB, you still have 1GB left that can be used for another application (or applications).

	For services and APIs, you are given an allocation of 10.  An example of a service is a PostgreSQL database service which you will create later. 

	<br>
	
1. The physical location (e.g., location of the data center) of the Bluemix servers that will host your application is referred to as Bluemix regions.  Currently there are three Bluemix regions: `United Kingdom`, `Sydney,` and `US South`.

	In this tutorial, you will be using the `US South` region.  However, in an actual deployment, you may choose any of the available regions.  If you will be deploying several applications, these applications may be deployed in different region (e.g., your first application is in `United Kingdom`, while the second one is in `US South`).  However, the total consumed resources in these regions should not exceed the allocation given to your organization (e.g., 2GB memory and 10 services)

	<br>
	
1. Make sure that the current region used by your Bluemix account is `US South` by  clicking the `Person` icon on the upper-right corner of your account.  

	>Once you select `US South` it is possible that you will be prompted to create a space.  If you are prompted, enter a space named `dev`.  This purpose of a space is explained in the next step.

1. Under the `US South` region, you may deploy one ore more applications.  

	As the number of applications you deploy in a region increases, the harder it is to manage your applications.  To help manage them, applications are grouped together in spaces.  As an example, you may create spaces which groups applications belonging to the same phase.  For example, you may create a space called `dev` and deploy all of your applications that are still under development phase.  You may create a second space called `prod` for applications that are already in production.

	Another way to utilize spaces is to group applications based on projects.  For example, you may create a space called `proj1` for all projects belonging to project 1.

	<br>

1. Verify if you have a `dev` space at the left side of your dashboard.  If there is no `dev` space, create one by clicking the `Create a Space` link. 

	<br>

####Explore the Bluemix Catalog

1. In the menu, click `CATALOG`.  The Bluemix Catalog shows the different services and APIs, as well as runtimes and containers that you may create.

1. Scroll down until you see `PostgreSQL by Compose`.  PostgreSQL is a type of relational database.  

	In Bluemix, there are services that are very similar.  As an example, in this tutorial, you will be creating a PostgreSQL service.  However, the PostgreSQL service that you will be using is NOT `PostgreSQL by Compose`.

	<br>
	
1. Scroll down further in the `CATALOG` page until you see the `Bluemix Labs Catalog` link.  Click this link.

1. Scroll down until you see `postgresql`.   The `postgresql` service and the `PostgreSQL by Compose` service are very similar (i.e., both are PostgreSQL services).  In this tutorial, you will be using `postgresql`.  When you are asked to create a PostgreSQL service in later steps, make sure to use `postgresql` and not `PostgreSQL by Compose`.

	<br>
	
1. Leave your Bluemix account open on the browser.  You will use this again later.

	<br>

####Install the Cloud Foundry (`cf`) tool.

Cloud Foundry is an open-source platform as a service cloud technology.  Bluemix is based from Cloud Foundry.  Cloud Foundry has a command-line tool called `cf` that is used to deploy applications in cloud foundry-based environment.  Since Bluemix is based on Cloud Foundry, the same `cf` tool can be used to deploy applications in Bluemix.


1. Go to the Cloud Foundry `cf` tool [repository](https://github.com/cloudfoundry/cli/releases).

1. Download the appropriate installer (not the binary).

1. Install using the default options.

1. To test if installation is successful, open a terminal window and issue the following command:

	```text		
	> cf
	```

	**Output:**
		
	```
	NAME:
	   cf - A command line tool to interact with Cloud Foundry
	
	USAGE:
	   [environment variables] cf [global options] command [arguments...] [command options]
	   :
	   :
	```
	
	You should see the help screen of `cf` (see above example).

	<br>


####Copy Sample Application
You will download a copy of a sample application that you will deploy in your Bluemix account.


1. Create the directory `bluemixtemp` in the root directory.  Create a subdirectory `myfirstapp` in `bluemixtemp`.

1. Download [PostgreSQLUpload.war](https://github.com/ibmjstart/bluemix-java-postgresql-uploader/releases/download/v1.1/PostgreSQLUpload.war) and save it in the `myfirstapp` subdirectory.

<br>


####Deploy Sample Application in Bluemix using the `cf` tool.

1. Open a terminal window and go to the `myfirstapp` subdirectory.

1. Login to your Bluemix account using the `cf` tool.

	```text
	> cf login -a https://api.ng.bluemix.net -s dev
	```
	
	>When asked for a username (e-mail address) and password, enter the username and password of your Bluemix account.

	**Output:**

	```
	API endpoint: https://api.ng.bluemix.net
	
	Username> -----
	
	Password>
	Authenticating...
	OK
	
	Targeted space dev
	
	API endpoint: https://api.ng.bluemix.net (API version: 2.40.0)
	User:         -----
	Org:          -----
	Space:        dev
	```
	
	The `-a` switch allows you to specify the URL of the Bluemix region.  In this tutorial, you are using the `US South` region.  The URL of this region is `https://api.ng.bluemix.net`.

	The `-s` switch allows you to specify the space where you will deploy the application.  In this tutorial, you will be deploying the application in the `dev` space you created earlier.

	<br>
	
1. Upload the sample application to your Bluemix account.

	```text
	> cf push myfirstapp-<your_name> -m 256M -p PostgreSQLUpload.war
	```

	**Example:**
		
	```text
	> cf push myfirstapp-pong -m 256M -p PostgreSQLUpload.war
	```

	**Output:**
		
	```text
	:
	:
	     state     since                    cpu    memory           disk
	#0   running   2016-01-14 08:00:15 AM   0.8%   185.1M of 256M   180.9M	
	```

	In the example above, the name of the application is `myfirstapp-pong`.  Replace `pong` with your name.  The name of your application will be concatenated with string `.mybluemix.net` and this will be the URL of your application.  As an example, the application `myfirstapp-pong` is accessible using the URL http://myfirstapp-pong.mybluemix.net.

	>**IMPORTANT:** If you encounter an error message `host is taken`, it means that the application name you specified has already been used by another Bluemix user.  Issue the `cf push` command again but change the name of your application.  In the example above, instead of `myfirstapp-pong`, it can be `myfirstapp-pong2`.

	The `-m` switch allows you to specify the memory allocation of your application.

	The `-p` switch allows you to specify the location of the file containing the sample application.

	<br>
	
1. Go back to the browser tab containing your Bluemix account.  In the menu, click `DASHBOARD`.  

	The `Applications` section of your dashboard shows a widget representing the application `myfirstapp-<your_name>` you deployed earlier.

	The widget shows some information regarding the application:
	- **status**: your application is in a running state
	- **route (a.k.a. URL)**: `myfirstapp-<your_name>.mybluemix.net`
	- **runtime**: Liberty for Java (this is the type of web server hosting your application)
	- **list of services the app is bound to**: currently empty

	Take note that aside from logging in using the `cf` tool, the only other command you issued earlier is `cf push` which deployed your application in your Bluemix account.  You NEVER explicitly created/set-up a web server (e.g., Liberty for Java web server) to host your application.  When you issued the `cf push` command, Bluemix determined that you want to deploy a Java-based web application and automatically created the necessary web server (which we refer to in Bluemix as **runtime**) to host your application.

	Currently the **list of services the app is bound to** is empty but when you add a PostgreSQL service later you will see an icon added in the widget representing the service.

	<br>
	
1. Click the widget of your application to see its overview.

	The overview shows the following information:
	- **instances**: 1
	- **memory quota**: 256MB
	- **activity log**
	- **routes**: `myfirstapp-<your_name>.mybluemix.net`

	Adjusting the **instances** value allows you to perform horizontal scaling.  For example, if you observe that a deployed application cannot handle anymore the number of users accessing the application then you may increase the number of instances.  However, take note that for every additional instance you add, your application will consume an additional memory space equal to the **memory quota**.  For example, if you have 2 instances and 256MB as the memory quota, then your application consumes 512MB of memory.

	Adjusting the **memory quota** value allows you to perform vertical scaling.  

	<br>
	
1. On the left pane, click the `Environment Variables` link.

	Bluemix has a system-defined environment variable called `VCAP_SERVICES`.  Currently, the value of `VCAP_SERVICES` is empty.   You will see later the purpose of `VCAP_SERVICES`.
	
	<br>

1. Open another browser tab (do not close the browser tab containing your Bluemix account).  Go to `http://myfirstapp-<your_name>.mybluemix.net` to verify that the sample application is successfully deployed.

	> If you encounter a `404 Not Found: Requested route ('-----.mybluemix.net') does not exist`, it may mean any of the following:
	>a. you typed the wrong URL (**solution:** double check the URL)
	>b. your application is not yet running (**solution:** wait for your application to run, refer to the sample output above)
	>c. your application failed to run (**solution:** look at the error message and issue again the `cf push` command)

		
	The sample application allows you to upload a text file in a PostgreSQL database.  You will test if this sample application is correctly running.

	<br>

1. Click the `Browse` button of the sample application and choose any text file.
	> Make sure it is a text file and not a binary file.
	> If you don't have any text file, just create one and place at least 3 lines of text.
	
	<br>
	
1. Click the `Upload` button.  If the upload operation is successful, the contents of the text file will be saved in a PostgreSQL database.  

	HOWEVER, since you have not created any PostgreSQL database yet, you encountered the error `No PostgreSQL service URL found. Make sure you have bound the correct services to your app.`.  You will fix this error by creating a PostgreSQL server later.

	<br>
1. Close the browser tab containing the sample application.

	<br>
	
####Add a PostgreSQL Service and Bind it to the Sample Application

1. Go back to the browser tab containing your Bluemix account.  On the left pane, click the `Overview` link. 
	
1. Click the `ADD A SERVICE OR API` link.  You will be redirected to the `Catalog` page. 

1. Look for the `postgresql` service and click it.

	>**VERY IMPORTANT:**
	The `postgresql` service that you will use in this tutorial is NOT the `PostgreSQL by Compose`.  
	<br>
	In the `Catalog` page, scroll down in the `CATALOG` page until you see the `Bluemix Labs Catalog` link.  Click this link.
	<br>
	Look for the service named `postgresql` and click this service.

	<br>

1. In the `Service name` text box, type `postgresql-myfirstservice`.

1. Click the `CREATE` button.

1. When asked to restage your application, click the `RESTAGE` button.  Wait for your application to restage.

1. Open another browser tab (do not close the browser tab containing your Bluemix account).  Go to `http://myfirstapp-<your_name>.mybluemix.net` to test if the sample application can already connect to the created PostgreSQL service.

	<br>

1. Click the `Browse` button of the sample application and choose any text file.

	
1. Click the `Upload` button.  

	This time, the upload is successful.  You will see the contents of the text file displayed on the page.   The sample application is programmed to display the contents of the PostgreSQL service.  Since the contents of the text file is displayed on the page, the contents are successfully saved in the PostgreSQL service.

	<br>

####Analyze How the Sample Application communicates with PostgreSQL Service

Reviewing the procedure above, you did two important tasks: (1) deployed the sample application and (2) created a PostgreSQL service.

It seems impossible for the sample application to be able to communicate with the PostgreSQL service since you have not updated the sample application to use the credentials of the service (e.g., username, password, IP address, port no., etc.).  

However, as demonstrated, you were able to make the sample application to communicate with the service.  This was accomplished by coding the sample application such that the database credentials needed to create the connection string are not hard coded.  Instead it uses the credentials found in the environment variable `VCAP_SERVICES` which was originally empty earlier.


1. Go back to the browser tab containing your Bluemix account.  On the left pane, click the `Environment Variables` link. 

	Recall that earlier `VCAP_SERVICES` is empty.  However, it now contains a value similar to the one you see below:

	```text
	{
	   "postgresql-9.1": [
	      {
	         "name": "postgresql-myfirstservice",
	         "label": "postgresql-9.1",
	         "plan": "100",
	         "credentials": {
	            "name": "d318bc5931cd540b694de846b004b3955",
	            "host": "198.11.228.48",
	            "hostname": "198.11.228.48",
	            "port": 5433,
	            "user": "u536f68cf365d4ee6a3a3e00cbf209c53",
	            "username": "u536f68cf365d4ee6a3a3e00cbf209c53",
	            "password": "pdf27393fb94d4d45913cd54751d346ad",
	            "uri": "postgres://u536f68cf365d4ee6a3a3e00cbf209c53:pdf27393fb94d4d45913cd54751d346ad@198.11.228.48:5433/d318bc5931cd540b694de846b004b3955"
	         }
	      }
	   ]
	}
	```

	The value above contains the credentials of the PostgreSQL service.  This value was produced when you created the service earlier.  

	Recall that you clicked the `ADD A SERVICE OR API` link earlier then created the PostgreSQL service.  Adding a service (or API) does two things:
		- create a service
		- bind the service to the application

	Binding the PostgreSQL service to the application simply instructs Bluemix to share the credentials of the PostgreSQL service to the sample application.  The credentials are shared by placing the values of the credentials to `VCAP_SERVICES`.

	However, it needs to be emphasized that even if the credentials are shared through the `VCAP_SERVICES`, this sharing is useless unless the application explicitly use the `VCAP_SERVICES` environment variable.

	You will examine the source code inside `PostgreSQLUpload.war` to see how `VCAP_SERVICES` is used.

	<br>
	
1. If you extract the contents of `PostgreSQLUpload.war` you will see the subdirectory `WEB-INF/classes/com/ibm/bluemix/samples`.  This contains several `.java` files including `PostgreSQLClient.java`.

	> You don't need to extract the contents of the `.war` file since the contents of the needed files are shown below.  However, you may use tools such as `7Zip` if you want to extract the contents.
	
	`PostgreSQLClient.java` has a method called `getConnection`:
	
	```java
		private static Connection getConnection() throws Exception {
			Map<String, String> env = System.getenv();
			
			if (env.containsKey("VCAP_SERVICES")) {
				// we are running on cloud foundry, let's grab the service details from vcap_services
				JSONParser parser = new JSONParser();
				JSONObject vcap = (JSONObject) parser.parse(env.get("VCAP_SERVICES"));
				JSONObject service = null;
				
				// We don't know exactly what the service is called, but it will contain "postgresql"
				for (Object key : vcap.keySet()) {
					String keyStr = (String) key;
					if (keyStr.toLowerCase().contains("postgresql")) {
						service = (JSONObject) ((JSONArray) vcap.get(keyStr)).get(0);
						break;
					}
				}
				
				if (service != null) {
					JSONObject creds = (JSONObject) service.get("credentials");
					String name = (String) creds.get("name");
					String host = (String) creds.get("host");
					Long port = (Long) creds.get("port");
					String user = (String) creds.get("user");
					String password = (String) creds.get("password");
					
					String url = "jdbc:postgresql://" + host + ":" + port + "/" + name;
					
					return DriverManager.getConnection(url, user, password);
				}
			}
			
			throw new Exception("No PostgreSQL service URL found. Make sure you have bound the correct services to your app.");
		}
	```

	The method parses the contents of `VCAP_SERVICES` and looks for the credentials of the PostgreSQL service.  This approach allowed the sample application to dynamically get the credentials of the service instead of hard coding it.

	The use of `VCAP_SERVICES` explains how the sample application is able to connect to the PostgreSQL service.  However, this did not explain how a database table (that stores the content of text files) was created.  Recall that you never performed a task that explicilty created a table in the PostgreSQL service.

	`PostgreSQLClient.java` has another method called `createTable`:
	
	```java
		private void createTable() throws Exception {
			String sql = "CREATE TABLE IF NOT EXISTS posts (" +
							"id serial primary key, " +
							"text text" +
						 ");";
			Connection connection = null;
			PreparedStatement statement = null;
			
			try {
				connection = getConnection();
				statement = connection.prepareStatement(sql);
				statement.executeUpdate();
			} finally {			
				if (statement != null) {
					statement.close();
				}
				
				if (connection != null) {
					connection.close();
				}
			}
		}
	```

	The `createTable` method allowed the tables to be programmatically created (i.e., no need for you to manually create the table).  In usual practices, the database administrator creates the tables.  However, the sample application was designed to programmatically create the table to easily demonstrate the use of a service in Bluemix.

	<br>
	
####Delete the Sample Application and PostgreSQL Service
You may delete applications and services that you don't anymore need.  This will free up some resources which is essential to accommodate new applications and services you want to deploy in the future.

1. Go back to the browser tab containing your Bluemix account.  In the menu, click `DASHBOARD`.  

	Notice that the widget of the sample application got updated.  An icon was added in the widget.  If you will mouse hover on the icon, you will see that the icon refers to the PostgreSQL service you created earlier.  The presence of this icon in the widget of the sample application  means that the service is bound to the application.  

	Take note that it is possible that you may have additional services bound to the same application.

	In addition, notice that a widget for the PostgreSQL service is also available.  It also has an icon that refers to the sample application.  The presence of this icon in the widget of the PostgreSQL service means that the application may use this service. 

	Take note that it is possible that you may have additional applications bound to the same service.
	
	<br>
 
1. Click the `gear` icon in the widget of the sample application.

1. Click the `Delete App` entry.  In the `Services` tab, make sure that the PostgreSQL service is selected.  In the `Routes` tab, make sure that the route (i.e., URL) is selected.

1. Click the `DELETE` button.

<br>

####End of Tutorial


