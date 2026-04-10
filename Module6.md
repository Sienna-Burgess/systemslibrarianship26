# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026
###Module 6: Library Website Project
---


---

 **Section 11: Install WordPress**
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
				- create user 'wordpress'@'localhost' identified by 'xxxxxxxx';

		- Then create the database wordpress:
				- create database wordpress;

		- 	Then grant all privileges:
				- grant all privileges on wordpress.* to 'wordpress'@'localhost';		

		- Then show databases:
				- show databases;

		- Then exit:
				- \q		


- Step 4: Set up wq-config.php

		- When we started making our barebones ILS, we created a file named [login.php], that contained the name of the database [opacdb], the name of the database user [opacuser], and the user's password. 
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
					



**Section 12: Install Omeka**
- Goal: The goal in this section is to install Omeka Classic, 
- Context: In this module, we will learn how to install Omeka, using the same basic process to download, install, and configure Omeka. This will be done by utilizing our previous knowledge from sections 5 and section 11. 
- Prerquisite Steps:
			- IS YOUR STUFF UP TO DATE? Better double check:
					- sudo apt update ; sudo apt upgrade -y
			- Check that you have the right PHP extension snd mySQL:
					- php --version		
					- mysql --version
			- Install ImageMagick. This is a suite of utilities that works with photo files. Omeka uses ImageMagick to create thumbnail images of photos uploaded to the digital library. 
					- sudo apt install imagemagick
					- Press y, to install.
			- Enable Apache [mod_rewrite]. This is an Apache module use to rewrite URLs. Omeka uses this to create use friendly URLs for items and collections in its digital libraries. 
					- sudo a2enmod rewrite
			- Then we were instructed to restart Apache after enabling [mod_rewrite]:
					- sudo systemctl restart apache2

	- Now it is time to do the real deal stuff. 						

- Steps: 
		- Firstly, we hope/pray/cry or whatever is needed, for this to go smoothly. 

		- Now, we really begin. We need to log in to MySQL root user with [sudo]:
				- sudo mysql -u root
		- Next, we will make a new database in MySQL for the Omeka installation. On GOD we cannot add it to our WordPress one, so, we making a new one:
				- create user 'omeka'@'localhost' identified by 'xxxxxxxx';
					- The 'xxxxxx' are whatever we decided the password would be. Don't be forgetting it.		
		- A step I forgot was to add a database, so we will do that now. In MySQL,create the database:
				- create database digital_library;
		- Next, we need to grant all privilages:
				- grant all privilages on digital_library.* to 'omeka'@'localhost';		 
		- Then exit out:
				- \q
				
		- Next, we will use [wget] from our server to download the latest Omeka Classic release as a Zip file and extract it in [/var/www/html].
		- First, go to https://omeka.org/classic/download/ and download.
		- Next, we gotta download and extract the WordPress software. This will be in a [zip] file. While we only download one file, when we extract it with the [unzip] command, the extraction will result in a new directory that has multiple files and subdirectoried. 
		  To do this: 
		  	- cd /var/www/html
		  	- sudo unzip omeka-3.2.zip
		- Next, to rename the file, we will:
			- ls
		- Then, we will use the following command:
			- sudo mv omeka-3.2 omeka
		- Then, to double check we got that right, lets pull that stuff up again:
			- ls

	Alright, now our next step is to open up that extracted durectory. In there, we will find [db.ini] and add our new database creditials.
		- First, we will need to open the omeka directory:
			- cd omeka/
		- Then, we will list off the files:
			- ls
		- Cool, we see the [db.ini] file, which is the one we want to open. So, use the following command:
			- 	sudo micro db.ini
		- Next, all those "XXXXX" we gotta put our info in. No sweat! 
			- host 		= localhost
			- username 	= omeka
			- password	= thats a secret fr
			- dbname	= digital_library
		- Save that stuff and exit. 
		- Then we will run the following commands to give Apache group write access on all files. 
			- cd /var/www/html/omeka
			- sudo chmod -R g+w *
		- Restart Apache:
			- 	sudo systemctl restart apache2
		- Restart MySQL:
			- sudo systemctl mysql

		-FINALLY, I forgot to enable the mod_rewrite,, so first we will get to that spot:
			- sudo a2enmod rewrite
		- Then we will open the Apache config:
			- sudo micro /etc/apache2/apache2.conf
		- Then, we will find the <Directory /var/www/>
			- Here, we will change it to:
				- AllowOverride All		
			- Save and exit. 
		- Restart Apache:
			- sudo systemctl restart apache2
		- Restart MySQL:
			- sudo systemctl mysql


- Results: I was able to utilize what I have learned in the past to create, install, and set up Omeka, using my prior lessons on doing the same with our bare bones ILS and WordPress.
- Verification: http://34.171.244.152/omeka/install/
- Notes/Reflection: I had to go back and forth a few times in this process because I had forgotten to do certain steps. But overall, I am feeling a bit more confident about working though this stuff. I do have some anxieties on what our final project should look like but as we move forward, I am looking forward 
to demonstrating more of what I have learned. I think the big thing I am still needing to do is to slow down, only a little bit more because this process went overall really smoothly, but to make sure I am paying attention to the details. 
I will say, I did really enjoy putting more of a spin on my notes. It added some humor but also it became like a conversational checklist for myself.


