# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026
###Module 6: Library Website Project
---


---

 **Module 6: Install WordPress**
- Goal: The goal is to install WordPress which is a free and open source CMS (Content Manahement System). 
- Context: In this module, we will learn how to install WordPress by downloading the most recent version from WordPress.org.
- Steps:

		- First we need to verify that we are using the right PHP version:
				- php --version

		- Then, we will double check out MySQL verson:
				- mysql --version

		- Next, the output from [php --version] and [myswl --version] should confirm that out software meets or exceeds those recommendations. Our exact versions may differ based on our Ubuntu relaease and package updates. To check our Ubuntu, use:
				- cat /etc/issue.net
				
		- Before we proceed, update that girl:
				- sudo apt update ; sudo apt upgrade -y ; sudo apt autoremove -y ; sudo apt clean

		- Next, we need to add more PHP modules to our system to let WordPress operate at full functionality:
			- sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl

		- Then, let's restart Apache2 and MySQL:
			- sudo systemctl restart apache2
			- sudo systemctl restart mysql


- Step 2: Download and Extract that Stuff
		- Next, we gotta download and extract the WordPress software. This will be in a [zip] file. While we only download one file, when we extract it with the [unzip] command, the extraction will result in a new directory that has multiple files and subdirectoried. 
		  To do this: 
		  	- cd /var/www/html
		  	- sudo wget https://wordpress.org/latest.zip
		  	- sudo apt install unzip
		  	- sudo unzip latest.zip

		  - Then once we unzip the [latest.zip] file, we can delete it to free some space:
		  	- sudo rm latest.zip


- Step 3: Create the Database and a User
		- 
		- First we will Log in as the MySQL root user with [sudo]:
				- sudo mysql -u root

		- Then we are going to create a user and create a password. The "xxxxxx" are in place for the password you created.
				- create user 'wordpress'a'localhost' identified by 'xxxxxxxx';

		- Then create the database wordpress:
				- create database wordpress;

		- 	Then grant all privileges:
				- grant all privileges on wordpress.* to 'wordpress'@'localhost';		

		- Then show databases:
				- show databases;

		- Then exit:
				- \q		


- Step 4: Set up wq-config.php

		- When we started making our barebones ILS, we created a file named [ligin.php], that contained the name of the database [opacdb], the name of the database user [opacuser], and the user's password. 
		  WordPress will follow a similar process, but instead, of [login.php], it uses a file called [wp-config.php].
	  	- To get started, we will change into the [wordpress] directory:
	  		- cd wordpress/

	  	- Next, we will copy and rename the [wp-config-sample.php] file to [wp-config.php]:
	  		- sudo cp wp-config-sample.php wp-config.php
	  		
		- Then we will want to open and edit the file and add our WordPress database name, user name, and password in the fields for:
			- sudo micro wp-config.php
				- DB_NAME: wordpress
				- DB_USER: wordpress
				- DB_PASSWORD: (In notes).

		- Next, if WordPress cannot write files due to permissions in this lab environment, add the following line at the end of [wp-config.php]:
				- define('FS_METHOD','direct');

				






- Results: Now we got a wordpress site called libcat.sienna
- Verification: http://34.171.244.152/wordpress/
- Notes/Reflection: As we have moved through this course, I am starting to feel a lot more confident in going through these processes. 
					For an example, when we first started this course, I was always worried about not completing a lecture in one go because I was not confident I would be able to get back to the same spot as before.
					Now, like with this lesson, I got through most of it but, I had to stop after I completed Step 3, and came back to complete the lesson the next day. 
					While this is a small win, it is one that I am proud of because I had very little background in this before. 
					Another thing that I am proud of is that it takes less time for me to catch my mistakes. In this lesson, I has accidentally mistyped when copying and renaming the [wp-config-sample.php] file. While I had already duplicated it, I noticed when we open the [wp-config.php] file that I had nothing in there. 
					All of these things are small wins, but they are huge for me because I lack this type of experience. 
					

