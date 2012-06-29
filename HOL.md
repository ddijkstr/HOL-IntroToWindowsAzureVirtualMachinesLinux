<a name="handsonlab" />
# Introduction to Windows Azure Virtual Machines (Linux) #

---

<a name="Overview" />
## Overview ##

Using Windows Azure as your IaaS platform will enable you to create and manage your infrastructure quickly, provisioning and accessing any host ubiquitously. Grow your business through the cloud-based infrastructure, reducing the costs of licensing, provisioning and backup.

In this Hands-On Lab, you will learn how to use the Windows Azure IaaS platform to provision Linux based VMs in the cloud and manage it remotely. 

<a name="Objectives" />
### Objectives ###

In this hands-on lab, you will learn how to: 

- Create Linux virtual machines in Windows Azure
- Install and configure an Apache web server with MySql 
- Install and configure Drupal CMS

<a name="Prerequisites" />
### Prerequisites ###

- A Windows Azure subscription with the Virtual Machines Preview enabled - you can sign up for free trial [here](http://bit.ly/WindowsAzureFreeTrial)

> **Note:** This lab was designed to use **OpenSUSE** linux distribution when creating the new Virtual Machine in Windows Azure.

---
 
<a name="Exercises" />
## Exercises ##

This hands-on lab includes the following exercises:

1. [Creating a Linux VM in Windows Azure](#Exercise1)

1. [Installing and Configuring the VM](#Exercise2)

1. [Installing and Configuring Drupal](#Exercise3)
 
Estimated time to complete this lab: **45 minutes**.


<a name="Exercise1" />
### Exercise 1: Creating a Linux VM in Windows Azure ###

In this exercise, you will learn how to provision Linux Virtual Machines in the Azure portal.
	
<a name="Ex1Task1" />
#### Task 1 - Creating a New Linux Virtual Machine ####

1. Open Internet Explorer and browse [https://manage.windowsazure.com](https://manage.windowsazure.com) to enter the Windows Azure portal. Then, log in with your credentials.
1. In the menu located at the bottom, select **New | Virtual Machine | From Gallery** to start creating a new virtual machine.

	 
	![Creating a new Virtual Machine](images/creating-a-new-virtual-machine.png?raw=true)

	_Creating a new Virtual Machine_
 
	1. In the **VM OS Selection** page, click **Platform Images** on the left menu and select the **OpenSUSE** OS image from the list. Click the arrow to continue.	

		![Selecting OpenSUSE from the image list](images/creating-a-vm-suse.png?raw=true)	
	 
		_Selecting OpenSUSE from the image list_

	1. In the **VM Configuration** page, enter the **Virtual Machine Name**, set the **New User Name** to **Administrator**, the **Password** and the **Size** for the VM. Click the **right arrow** to continue.

		![Configuring a Custom VM](images/creating-a-vm-configuration.png?raw=true)	
	 
		_Creating a VM - Configuration_
 
		>**Note:** It is suggested to use secure passwords for admin users, as Windows Azure virtual machines could be accessible from the Internet knowing just their DNS.

		>You can also read this document on the Microsoft Security website that will help you select a secure password:  [http://www.microsoft.com/security/online-privacy/passwords-create.aspx](http://www.microsoft.com/security/online-privacy/passwords-create.aspx)
 
	1. In the **VM Mode** page, select **Standalone Virtual Machine**, enter the **DNS Name**, you can automatically generate a new Storage Account or select one you already own. Then, select the **Region/Affinity group/Virtual Network** value (by default, West US) and select the **subscription**. Click the **right arrow** to continue. 

		![Configuring a Custom VM, VM Mode](images/creating-a-vm-vm-mode.png?raw=true)
	 
		_Creating a VM - VM Mode_
 
	1. In the **VM Options** page, click the button to create a new VM.

		![Creating a VM - VM Options](images/creating-a-vm--vm-options.png?raw=true "Creating a VM - VM Options")

		_Creating a VM - VM Options_
 
1. In the **Virtual Machines** section, you will see the VM you created with a _provisioning_ status. Wait until it changes to _Running_ in order to continue with the following step.

	![Creating Linux VM](images/creating-linux-vm.png?raw=true)
	 
	_Creating Linux VM_

1. Now, you will create the endpoints required to manage the VM. To do this, select the VM to go to the **Dashboard** page and then click **Endpoints**.

1. Click **Add Endpoint**, select **Add Endpoint** option and then click the **right arrow** to continue.

	![Adding a new Endpoint](images/adding-a-new-endpoint.png?raw=true "Adding a new Endpoint")

	_Adding a new Endpoint_

1. In the **Specify endpoint details** page, set the **Name** to _web_, the **Protocol** to _TCP_ and the **Public Port** and **Private Port** to _80_.

	![New Endpoint Details](images/new-endpoint-details.png?raw=true "New Endpoint Details")

	_Specify Endpoint Details_
	
	> **Note:** This endpoint enables the port 80 that will be used by the Apache Server in the following tasks.

<a name="Ex1Task2" />
#### Task 2 - Verification: Connecting to the Virtual Machine by using a SSH client ####

Now that you have provisioned and configured a Linux Virtual Machine, you will connect by using an SSH client.

>**Note:** You can download Putty, a free SSH client for Windows, here: [http://www.putty.org/](http://www.putty.org/)


1.	In the Windows Azure Portal, select the Linux VM from the list to enter the **Dashboard**. Take note of the **SSH Details** field at the "quick glance" section, which is the public address you will use to access and connect to the virtual machine using the SSH client.

    ![SSH Endpoint](images/ssh-endpoint.png?raw=true "SSH Endpoint")

    _SSH Endpoint_

1. Open the Putty client (or any other SSH client) and create a new connection to the VM, using address and port from the previous step.

	![Connecting to the Linux VM via Putty Client](images/connecting-to-the-linux-vm-via-putty-client.png?raw=true)
	 
	_Connecting to the Linux VM via Putty Client_

	> **Note:** You can verify the public port for the SSH connection in the **Endpoints** section of the VM.
 
1. Use the VM credentials to login.

	![Logging in to the Linux Virtual Machine](images/logging-in-to-the-linux-virtual-machine.png?raw=true)

	_Logging in to the Linux Virtual Machine_

1. Execute the following command to impersonate with **Administrator** rights.

	````Linux
	sudo su -
	````

	![Logging in with Administrator rights](images/logging-in-with-administrator-rights.png?raw=true)

	_Logging in with Administrator rights_

<a name="Exercise2" />
### Exercise 2: Installing and Configuring the VM ###

In this exercise, you will learn how to install and configure a Web Server in the Linux VM by using a SSH client. First, you will install the Apache Web server and the MySQL database server by using Yast2 application. Then, you will configure the VM and create an example database. 

>**Note:** If you have not run Exercise 1, make sure you have the following items ready before proceeding with Exercise 2:
  
> - A Linux Virtual Machine created in Windows Azure Portal.
> - A TCP Endpoint enabled in port 22.  
> - An TCP Endpoint enabled in port 80.

<a name="Ex2Task1" />
#### Task 1 - Installing and Configuring Apache and MySQL ####

In this task, you will install and configure an Apache HTTP Server and MySQL Database Management System.

1. Install **Yast2**. In the terminal, execute the following commands to install the required packages.

	````Linux
	zypper install yast2
	zypper install yast2-ncurses
	zypper install yast2-ncurses-pkg
	zypper install yast2-qt
	zypper install yast2-gtk
	zypper install yast2-packager
	zypper install yast2-network
	zypper install yast2-http-server
	````

1. To install the prerequisites for Drupal you will use **Yast2** to automatically install Apache and MySQL with their dependencies. In the terminal, execute **Yast2**. This will open the **Yast2** application.

	````Linux
	yast2
	````

1. Using the arrow keys from your keyboard, select **Software**, press the right arrow and select **Software Management** and hit **ENTER**.

	![YaST2 Control Center](images/yast2-control-center.png?raw=true)

	_Software Management in YaST2_

1. Press **ALT+F** to change the Filter type and select **Patterns**.

	![Selecting the Patterns Filter](images/selecting-the-patterns-filter.png?raw=true)

	_Selecting the Patterns Filter_

1. Scroll down the options until you find **Web and LAMP Server**. Press **ENTER** to select it, then press **ALT+T**, select **All listed packages...** and **Install All**.

	![Installing Web and LAMP Server](images/installing-web-and-lamp-server.png?raw=true)

	_Installing Web and LAMP Server_

1. Press **ALT+A** to start the installation. Press **ENTER** when prompted for confirmation.

1. Once the installation is completed, the program will take you back to the main menu. Enter again the **Software Management**. Type **php5-json** to search for this package and hit **ENTER** to select it. Press **ALT+A** to start the installation.

	![Installing PHP extensions](images/installing-php-extensions.png?raw=true)

	_Installing PHP extensions_

1. Back to the main menu, press the left arrow key and select **Network Services**. Press the right arrow key, select **HTTP Server** and press **ENTER**.

	![Configuring HTTP Server](images/configuring-http-server.png?raw=true)
	
	_Configuring HTTP Server_

1. Follow the Wizard steps to complete the configuration using the default values.

1. Select **Network Services**. Press the right arrow key and select **HTTP Server** again.

1. Press **ALT+E** to enable the HTTP services and then press **ALT+F** to confirm the changes.

	![Enabling HTTP service](images/enabling-http-service.png?raw=true)

	_Enabling HTTP service_

1. Back to the main menu, press the left arrow key and select **System** from the menu options. Press the right arrow and select **System Services (Runlevel)** and press **ENTER**.

	![Configuring System Services](images/configuring-system-services.png?raw=true)

	_Configuring System Services_

1. Scroll down until you find **mysql** and press **ALT+E** to enable the service. Wait until the service is running. Press **F10** to save the settings.

	![Enabling MySQL service](images/enabling-mysql-service.png?raw=true)

	_Enabling MySQL service_

1. Press **ALT+Q** to exit **YaST2**.

<a name="Exercise3" />
### Exercise 3 - Installing and Configuring Drupal ###

In this exercise, you will install and configure the Drupal CMS in the Linux virtual machine. At the end of the exercise, you will be able to host a Drupal CMS website. 

>**Note:** Before starting this exercise, make sure you have completed Exercise 2.

<a name="Ex3Task1" />
#### Task 1 - Installing and Configuring Drupal ####

In this task, you will install and configure a Drupal portal on your Windows Azure Linux Virtual Machine. Additionally, you will create an empty database to be used by Drupal CMS.

1. Open the root websites folder and create a folder named **drupal** by executing the following.

	````Linux
	cd /srv/www/htdocs
	mkdir drupal
	cd drupal
	````

1.	Execute the following command to install **wget**.
	
	````Bash
	zypper install wget
	````

1. Download and extract **Drupal**.

	````Linux
	wget http://drupal.org/files/projects/drupal-7.10.tar.gz
	tar -xzf drupal-7.10.tar.gz --strip-components=1 
	````

	![Downloading Drupal](images/downloading-drupal.png?raw=true)

	_Downloading Drupal_

1. Copy the **default.settings.php** file, located in the **sites/default** directory, and rename the copied file to **settings.php**. Additionally, give the web server write privileges to the configuration file.

	````Linux
	cp sites/default/default.settings.php sites/default/settings.php
	chmod a+w sites/default/settings.php
	chmod a+w sites/default
	````

	![Creating the configuration file and granting permissions](images/creating-the-configuration-file-and-granting.png?raw=true)

	_Creating the configuration file and granting permissions_

1. To complete the installation, create an empty database for **Drupal** in **MySQL**. Execute the following:

	````Linux
	mysqladmin create drupaldb
	````

1. Execute **mysql**. At the MySQL prompt execute the following query. Replace **username** and **password** with your user account.

	````Linux
	GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES	
	ON drupaldb.*
	TO 'username'@'localhost' IDENTIFIED BY 'password';
	````

	![Granting permissions in MySQL](images/granting-permissions-in-mysql.png?raw=true)

	_Granting permissions in MySQL_
	
1. Open Internet Explorer and locate the virtual machine DNS Name. Browse to _https://[YOUR-DNS-URL]/drupal_ to complete Drupal installation.
 
	![Opening Drupal for the first time](images/opening-drupal-for-the-first-time.png?raw=true)

	_Opening Drupal for the first time_ 

	>**Note:**  You can find more details about Drupal configuration in the official documentation ([http://drupal.org/documentation/install/run-script](http://drupal.org/documentation/install/run-script)).

1. In the Set up Database page, enter the name of the database you have created in Task 1 (‘drupaldb’), and the **username** and **password**. 
	 
	![Opening Drupal for the first time(2)](images/opening-drupal-for-the-first-time2.png?raw=true)

	_Opening Drupal for the first time_
 
1. In the **Configure site** website, enter a user name, an e-mail address and a password to create a new **site maintenance account**.

	![Configuring a Drupal account](images/configuring-a-drupal-account.png?raw=true)
	 
	_Configuring a Drupal account_

1. Click the **Visit your Web site** link to verify that the Drupal home page loads. 

	![Drupal CMS home page](images/drupal-cms-home-page.png?raw=true)
	 
	_Drupal CMS home page_
	
---