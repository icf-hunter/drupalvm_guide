# drupalvm_guide
a how-to guide on setting up drupalvm and the associated config.yml file


DrupalVM steps

arrows (->) are for information
dashes (-) are for actions

## Installation
Required software:  
VirtualBox :: https://www.virtualbox.org/  
Vagrant    :: https://www.vagrantup.com/  
Git bash   :: https://git-scm.com/downloads  

1. go to https://www.drupalvm.com/, download or clone the vm (I placed it in my home directory for easy access but it doesn't matter where you put it)
	-for issues reference the quick start guide on this page
## Setup
2. copy the config.yml provided in this directory and paste it into the drupalvm directory you just created
	->this file overrides settings in default.config.yml

3. create a directory called 'd8-sandbox' inside drupalvm
	->this is where your drupal core setup will be saved
	
4. create a directory called 'projects' inside drupalvm
	->this is where your projects will be placed
	-you can now clone or pull a project into this directory with git bash
	
5. edit config.yml and find "virtual hosts added for projects"
	->there, you will find examples for drupal 8 (epamdu) and drupal 7 (smokefree) sites
	-duplicate, modify, or remove these as needed for your own sites
		->make sure that each documentroot property matches exactly with the a folder in your /drupalvm/projects directory
		->and drill down until you find the index.php or index.html (otherwise you will get a 403 Forbidden error when you try to navigate to the site)
## Vagrant Commands
6. navigate to /drupalvm and run the following command :
	vagrant up
	
7. Be patient. You may have errors when building, if so just run the following command to restart from where you left off :
	vagrant provision
	->note run this command anytime you make a change to config.yml or VagrantFile

8. once vagrant is done building drupalvm you can ssh into the box with the following command :
	vagrant ssh
	->note if on windows you need to use git bash as your ssh client (and make sure your enviornment variables are properly set up for git bash)
	
## quick check!
	-navigate to the drupalvm dashboard in your browser
		https://www.dashboard.drupalvm.dev
		-this will give you a list of your configured sites
	-or you can navigate to the blank drupal project you created in 'd8-sandbox'
		https://www.drupalvm.dev

# mysql database import
	for the purpose of clarity we will assume we have a *.sql file called 'example_db.sql' and we want to put that into a database called 'example'
	you can replace 'example' with the name of your database 
	
	a. make sure you are tunneled into drupalvm via ssh
		-if you are not, navigate to /drupalvm in terminal and run 'vagrant ssh'
	b. run the following commands to create a database called 'example' and then exit the mysql terminal :
		mysql -u root -p
		Enter password: root
		create database example;
		exit
	c. locate your *.sql file
	d. import example_db.sql into the database you just created 'example' with the following command
		mysql -u root -p example < C:/users/123456/drupalvm/projects/example/scripts/sql/example_db.sql;
		Enter password: root
	e. check to make sure everything worked as planned...
		mysql -u root -p
		Enter password: root
		show databases;
		use example;
		show tables;
	now that your database has been added you can go to your site! (You configured this name when you set 'servername:' in /drupalvm/config.yml)
		open browser > https://example.drupalvm.dev
	
	
### known problems
	if you are using drupal 7 your database settings will live in ./sites/default/settings.php
		locate the variable that names your database. To find it quickly you can search for the following term :
		'driver' => 'mysql'
		-rename the database to 'example' -or whatever you actually called your database when you created/imported it
		
	please include your own known problems as you come across them, and update the document accordingly.
