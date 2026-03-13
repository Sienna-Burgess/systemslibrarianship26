# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026                             
### Module 4: LAMP Server                                                                   

                                                                                                                                                        
### Section 7 Notes: Installing the Apache Web Server###                                    
 - Goal: To install and use the Apache web server, which is one of the most popular web serv
                                                                                            
 ***Command Line Notes:                                                                     
 - To figure out the specific package name, use:                                        
         apt search apache2 | head                                                          
 - The first package returned is what is needed:                                        
         apache2/noble-updates, noble securuty 2.4.58-1 ubuntu8.1amd64                      
 - To install apache, we begin with:                                                    
         cd /var/                                                                           
         ls                                                                                 
         sudo apt install apache2                                                           
 - To get information about the status of apache2 and make sure it is enabled and runnin
         systemctl status apache2                                                           
 - Text-Based Web Browser: the command line browser chosen was [elinks]. To instal, use:
         sudo apt install elinks                                                            
 - The loopback address is named localhost. There are two commands that allow us to chec
         elinks 127.0.0.1                                                                   
             or                                                                             
         elinks localhost                                                                   
 - To get the IP address, use:                                                          
         ip a                                                                               
         (This IP address will be located after the 2nd 'inet')                             
 - To get to the Apache2 default page, use:                                             
         elinks http://10.128.0.3                                                           
 - To see the default page outside of our VM, we need our servers public IP address, which is located



### Section 8 Notes: Installing and Configuring PHP###
- Note: PHP is a server-side programming language, which means it must be installed on the server.
		PHP- Hypertext Preprocessor

- Goal: Install and configure PHP on our virtual instances to work with the Apache webserver software.

- Command Line Notes: 
	- To see info about php, use:
		apt show php
	- To install PHP, use: 
		sudo apt install phplibapache2-mod-php
	- Once it is installed, we will confirm it is installed: 
		sudo systemctl restart apache2
	- Then to confirm the installed version, use:
		php -v
	- To check its status and see if there are any errors in the log output, use: 
		systemctl status apache2
	- To check that PHP is installed and working with Apache, we need to make a small PHP file in our web doc root. 
	  To do this, use: 
	  	cd /var/www/html/
	  *remember, [cd] ceated the document root.
	- Then, we create a file:
		sudo micro info.php
	- In that file, we added the following text:
		<?php
		phpinfo();,
		?>
	  *Note: each statement in a PHP function has to end with a semicolon! 
	- Next, we will visit that file from a browser (outside of the VM). To do this, search:
		http://34.171.244.152/info.php
	  *This will provide us with a page that provides system information about PHP, Apache and the server. 
	- Because of how our file system is lined up, we will want to configure it. Before we do thism we want to create a back-up. To do this, use: 
		cd /etc/apache2/mods-available/
	 Then:
	 	ls
	- Then we are going to edit the [dir.conf] file using: 
		sudo cp dir.conf dir.conf.bak
	  *This created a copy of the [dir.conf] file, incase we mess up, we have a copy of the OG file.
	- Next, we will open the [dir.conf] file with our editor of choice:
		sudo micro dir.conf
	- In our editor, we changed the line up to: 
		DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
	- When we do a configure change, we should check by using [apachectl]. So here, we do:
		apachectl configtest
	- Nextm we reload Apache:
		sudo systemctl reload apache2
	- Then, we will get the status:
		systemctl status apache2
		


                                        
                                                                                           

### Section 9: Installing and Configuring MySQL###
- Goal: To install, set up, secure, and configure the MySQL relational database to work with the Apache web server and the PHP programming language.

- Command Line Notes: 
	- To install the default MySQL server use: 
		sudo apt install mysql-server
	- To confirm which versions are available, use: 
		apt policy mysql-server
	- To confirm the version number, use: 
		mysql --version
	- Once it is installed, we can check if it is running and enable using the [systemctl] command:
		systemctl status my sql
	- To perform some security checks and create a secure, baseline configuration of MySQL, use:
		sudo mysql_secure_installation
	- To login to the database, use:
		sudo mysql -u root
	- When connecting to a MySQL server, and you want a list of the available databases, use: 
		mysql> [show databases;]
	- To exit the MySQL server prompt and return to the Bash shellm use:
		mysql> \q

- Create and Set Up a Regular User Account
	- To create an [opacuser], we use:
			- mysql> create user 'opacuser'@'localhost' identified by 'this_is_a_test123';

	- To clear the screen, use:
			- ctrl+l

- Create a Practice Database
	- To create a new database for the user account we just created, we will name it [opacbd] and set the character encoding to UTF-8 to support international characters:
		- mysql> create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;

	- Then run the MySQL [show] command to view the new database:
		- mysql> show databases;
	- Next, grat all privileges on the database to the user account [opacuser]
		- mysql> grant all privileges on opacdb.* to 'opacuser'@'localhost';

- Logging in as Regular User and Creating Tables Commandline Notes:
	- Next we need to pull information from our [~/.bashrc] file, to do so, use:
		- micro ~/.bashrc
	- Scroll to the bottom od the file and add the following:
		- export  MYSQL_PS1="[\d]>"
	- Then save and exit:
		- source ~/.bashrc
	- Then, we will log into our user, using:
		- mysql -u opacuser -p
		**Be mindful that when you are typing in the password, it does not show anything.
	- From the MySQL prompt, list the available databases and use the [use] vommand to switch to the new [opacdb] database using:
		- show databases;
			then:
		- use opacdb;
	- From here, we will be adding code to our [opacdb] database. The table will be called [books], to do this use:
		- create table books (
		-> id int unsigned not null auto_increment,
		-> author varchar(150) not null,
		-> title varchar(150) not null,
		-> copyright year not null,
		-> primary key (id)
		-> );
	- To confirm that the table was created use the following commands:
		- show tables;
		- describe books;
	* to clear the screen, use:
		- ctrl+l

- Adding Records into the Table
 insert into books (author, title, copyright) values
    -> ('Jennifer Egan', 'The Candy House', '2002'),
    -> ('Imbolo Mbue', 'How Beautiful We Were', '2021'),
    -> ('Lydia Millet', 'A Children\'s Bible', '2020'),
    -> ('Julia Phillips', 'Disappearing Earth', '2019');

	- Here you can see the entire record within patanthesis, and then each data for each field is in single quotes, and then each line ends with a comma.
	- The "\" in Children\'s escapes the quote

- Testing Commands
	- select author from books;
	- select copyright  from books;
	- select author, title from books;
	- select author from books where author like '%millet%';
	- select author, title from books where title not like '%e';
	- select * from books;
	- select title from books where author like '%mbue%';
	- alter table books add publisher varchar(75) after title;
	- describe books;
	- update books set publisher='Simon & Schuster' where id='1';
	- update books set publisher='Penguin Random House' where id='2';
	- update books set publisher='W. W. Norton & Company' where id='3';
	- update books set publisher='Knopf' where id='4';
	- select * from books;
	- delete from books where author='Julia Phillips';
	- insert into books
		(author, title, publisher, copyright) values
		('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
		('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');
	- select * from books;
	- select author, publisher from books where copyright < '2011';
	- select author from books order by copyright;
	- select author, copyright from books order by copyright;
	- select author, copyright from books order by copyright desc;
	- \q

- Install PHP and MySQL Support
	- To complete the connection between PHP and MySQL, we first install PHP support for MySQL:
		sudo apt install php-mysql
	- And then restart Apache and MySQL:
		sudo systemctl restart apache2
		sudo systemctl restart mysql

- Create PHP Scripts
	-  cd /var/www
	- sudo touch login.php
	- sudo chmod 640 login.php
	- sudo chown :www-data login.php
	- ls -l login.php
	- sudo nano login.php
		
