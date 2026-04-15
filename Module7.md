# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026                             
### Module 7: Koha                                                             

                                                                                                                                                        
### Section 13: Install the Koha ILS###                                    
 - Goal: To complete our Library infrastructure by installing the Koha ILS on our own by following written instruction. 
 - Context: Koha is an open source "library management system", also called an ILS (integrated library system). ILS is like a limited
 	library service plateform (LSP). LSP are a next generation ILS designed from the start to integrate electronic resources. An example
 	of a LSP that we use is OCLC's WorldCat!. 

                                                                                           
 ***Part 1: New Virtual Instance and Google Cloud Firewall                                                              
 - Before we can get started, we need to create a new virtual machine instance and configure the Google firewall to allow HTTP traffic to our Koha install.
 	The reason we need a new VI is because the one we have been using does not meet the RAM needs required by the Koha integrated library system. 

- To start creating our new VI: 
		- Click "Create instance"
		- Name: lis624koha
		- Region: us-central1 (Iowa)
		- Series: E2 Low cost, day-to-day computing
		- Machine Type: e2-medium (2 vCPU, 1core, 4 GB memory)
		- OS and Storage: Ubuntu 24.04 LTS, 20GB Hard Disk
		- Networking: Allow HTTP traffic
		- Network Tag: 
			- koha-staff-8080
			- koha-opac-8081

- Google Cloud Firewall:


	- Firewall Rule 1: Port 8080
		- First, we will go under "Related action" and click "Go to Firewall policies" (I could not find a hamburger icon).
			- Add name: koha-staff-8080
			- Add description: Open port 8080 for the Koha staff interface.
		- Next to Targets, click on Specified target tags.
			- In Target tags, add: koha-staff-8080
		- In the Source IPv4 ranges, allow access from anywhere: (0.0.0.0/0), to simplify setup. 
		- Click on Specified protocols and ports:
			- Click on TCP
			- Add 8080 in the Ports box
		- Click on Create. 	

	- Firewall Rule 2: Port 8081
		- Again, we will go under "Related action" and click "Go to Firewall policies"  (I could not find a hamburger icon).
			- Add name: koha-opac-8081
			- Add description: Open port 8081 for the Koha opac interface.
		- Next to Targets, click on Specified target tags.
			- In Target tags, add: koha-opac-8081
		- In the Source IPv4 ranges, allow access from anywhere: (0.0.0.0/0), to simplify setup. 
		- Click on Specified protocols and ports:
			- Click on TCP
			- Add 8081 in the Ports box
		- Click on Create. 


***Part 2: Micro, Install Koha Repo, Add Koha Repository
- Install Micro: 
			-  sudo apt install micro
- Now that we have created the virtual machine, and set those rules up, we will open it up.
	- In this machine, we will add tmux, which is a terminal multiplexer. This helps us because if we are disconnected while we are working, we can reconnect to our virtual machine, and run the following command to re-open our session:
			- tmux attach
	- To install tmux, we will run the following command:
			- sudo apt install tmux		


	- Now it is time to prepare our setver for the Koha installation. 
	- First, we need to update out local repository"
			- sudo apt update	
	- Then upgrade the server: 
			- sudo apt upgrade

	- To save some disk space and clear out the local repository of retrieved package files, we will combine and run the following commands:
			- sudo apt autoremove -y && sudo apt clean

- Add Koha Repository
	- By running the sudo apt apdate command, this tells Ubuntu to sync the local repository database with several remote repositories.
	- We can add repositories to sync with and to use to download software, including Koha ILS. 

		- First we will run the following three commands to set up the signing keys to ensure that we're downloading the authentic software: 
			- sudo apt install apt-transport-https ca-certificates curl
			sudo mkdir -p --mode=0755 /etc/apt/keyrings
			sudo curl -fsSL https://debian.koha-community.org/koha/gpg.asc -o /etc/apt/keyrings/koha.asc

		- Next, become the root user with the following command:
			- sudo su	
		- And paste the following into the terminal (be careful when pasting or else!):
			- tee /etc/apt/sources.list.d/koha.sources <<EOF
			Types: deb
			URIs: https://debian.koha-community.org/koha/
			Suites: 25.05
			Components: main
			Signed-By: /etc/apt/keyrings/koha.asc
			EOF
		- Then I leave that -ish:
			- exit	


                                                                                                         
***Part 3: Install MariaDB, Install Koha, Opening Ports and Apache2 Setup
- Install MariaDb
	- Now it is time to start installing MariaDB. The Koha ILS can use either MySQL relational database or Maria DB. 
	- Koha defaults to MariaDBm we will be using that. First we will install MariaDB, so that way Koha knows that it's using MariaDB as Koha is installed. 
		- sudo apt update
		- sudo apt install mariadb-server

- Install Koha
	- Next, update/sync the new repository with the Koha remote repository. So, just update that girl again:
		- sudo apt update
	- Now review the package info for Koha:
		- apt show koha-common
	- Now install the Koha ILS. This is a large package fr. To do this, run:
		- sudo apt install koha-common	

- Opening Ports
	- Like all ILS. Koha has a back end- staff interface, and a frontend- public interface. The back end/staff interface provides access to its modules, including cataloging, patron management, and more. 
	 The public interface provides access to the Koha OPAC. We will configure Loha and Apache to use different ports for the staff and public interfaces. Koha will access the staff interface through port 8080,
	 and the public interface through port 8081. This matches those firewall rules we set up.
	- To do this, we will edit the file at: /etc/koha/koha-sites.conf
	- Since this is an important configuration file, we will make a copy first: 
		- sudo cp /etc/koha/koha-sites.conf /etc/koha/koha-sites.conf.backup
	- Then, using micro, we will open it up: 
		- micro /etc/koha/koha-sites.conf	
		- At the end of the INTRAPORT= line, add 8080. At the end of the OPACPORT= line, add 8081. 
			- INTRAPORT= "8080"
			- OPACPORT= "8081"
		- ctrl+s
		- ctrl+q

- Apache2 Setup
- When we installed Koha, the Apache3 web server was installed too. However, Koha requires that we enable a few Apache modules with the a2enmod command. 
	- To do this, run:
		- sudo a2enmod rewrite cgi headers proxy_http
		sudo systemctl restart apache2

- Koha is already configured to listen on ports 8080 and 8081, but we also need to configure Apache2 to do so too!
- Again, because this is a big important configuration file, we gonna make a copy of it:
	- sudo cp /etc/apache2/ports.conf /etc/apache2/ports.conf.backup

- Then, use micro to oprn that girl: /etc/apache2/ports.conf
		- micro /etc/apache2/ports.conf
	- In the file, under Listen 80, add the following lines:
		- Listen 8080
		- Listen 8081


***Part 4: Create Koha Instance, Additional Apache2 Configurations, Koha Web Installer, and Publis OPAC
- Create Koha Instance
	- Koha comes with several copmmands to create and initiate the install. We will be using the koha-create command to create a Koha library called bibliolib. 
		- To do this we will run the following command and then restart Apache2:
			- sudo koha-create --create-db bibliolib
			- sudo systemctl restart apache2


- Additional Apache2 Configurations
	- The following commands configure Apache2 some more. /var/www/html typically serves as the web document root on a fresh and clean Apache2 install.
	 a2dissite 000-default will turn that off.While, a2enmod deflate turns on network compression. Meaning, data from the Koha server is compressed before it's sent to a client machine.
	 a2ensite bibliolib enables the new bibliolib library, which was already configured by Koha. 
	 	- Run the following: 
	 		- sudo a2dissite 000-default
	 		- sudo a2enmod deflate
	 		- sudo a2ensite bibliolib
	- Next, reload Apache2's new config and restart it:
			- sudo systemctl reload apache2
			- sudo system ctl restart apache2


- Koha Web Installer
	- Ayoooo, the backend is complete, so now we can complete the installation through a web installer. 
	- The first step here is to get the user name and password using the koha-passwd command. Make sure to save that information I swear to GOD:
		- sudo koha-passwd bibliolib
	- Then press enter to clear the screen.	

	- Next to get the public IP address of out machine to visit the Koha site and complete the installation. 
		- To get to the staff port of mine:
			- http://34.45.182.112:8080

		- Here we see a welcome screen with an onboarding process!!!!! Holy crap! Enter the username and password from the sudo koha-passwd bibliolib to get started!
		- Follow the steps, and be sure to complete the forms and accept the default options, but read through each step and refer to the documentation to understand what is being configured.
			- Library Code: 007
			- Library Name: Sienna Burgess Library


- Public OPAC
	- Once you've complete the install and onboarding process, you can visit your public OPAC. Note we add the the OPAC port at the end of our IP address this time:
		- http://34.45.182.112:8081
	- When the install and setup are complete, you can login with the admin credentials that you created during the web installer process, and you will have access to the staff interface.






- Results: I learned how to install and set up a Koha ILS install on a Linux server! Next, I gotta use my WordPress install to finalize my public facing library website. 
- Verification:
	- Koha Staff Port: http://34.45.182.112:8080
	- Koha Patron Port: http://34.45.182.112:8081
- Reflection: Through this process, I felt a lot more confident than I have before. A big thing that helped me through this was to print the instructions out. This helped me out a ton because
I am on a screen, pretty much all day, and all this semester, my eyes have felt fatigued by this. My job is computer heavy, and then after work, I am doing HW, which is also on a computer. 
I know it may sound silly, but by having this lesson printed out, I was able to rest my eyes inbetween sections because I would read, highlight and make notes before working through this module.
In this final section, it was really cool to see all the pieces start to come together. I am now nervous/excited to use WordPress to finalize the public facing side of the website. 
This time around, I can say that I didn't run into any problems. With printing out the instructions and taking my time, things actually went smoothly, which is a huge relief. 
I feel that this class built up to this assignment really well, so while working through this, it felt overall familiar. 
This component builds onto the foundational systems that we use in the library. It is important to understand the backend because it effects the front end operations. It also helps us to appreciate the hard work that our systems librarians do! 
