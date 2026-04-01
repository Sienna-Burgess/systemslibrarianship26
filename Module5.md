# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026
### Module 5: DIY Integrayed Library Systems
---
This page serves to document my work in LIS 624 via a Github repository. This will document my work from week 4 going forward. 
As time goes on in this course, this description will change and include additional information. 

---

 **Module 10: Creating a Barebines OPAC and Cateloging Module**
- Goal: To practice basic MySQL commands to better understand how relational databases are structured, queried, and managed.
  Then, we will create a very basic OPAC. The idea is tp understand how data from a relational database is retrieved and entered using LAMP tech.
  
- Commandline Notes:
	- First we start with opening MySQL:
		sudo mysql -u root
	- Then we will create a database called [DinnerDB]
		mysql> create database DinnerDB;
	- To confirm that the database is there, we use:
		mysgl> show databases;
	- Then, we will grant all privilages to [opacuser], (we could create another user but for this we wont):
		mysql> grant all privileges on DinnerDB.* to 'opacuser'@"localhost';	
	- Then exit out of the root MySQL user account:
		- \q

**Create Meals Table Command Line Notes**
	- Log in as opacuser:
		mysql -u opacuser -p
	- Enter password
	- Then we will check that the new [DinnerDB] database is visable to [opacuser]:
		show databases;
	- Then connect to the [DinnerDB] database:
		use DinnerDB;
	- To clear the screen, use:
		ctrl l (note, this is the letter 'L')	

- Create Meals Table:
	- Here we are going to create two tables in [DinnerDB], the first table is called [Meals] and the second table is called [Ingredients]. The second table will list the ingradiants and quantities needed to make the meals listed in the [Meals] table.
		- To create the [Meals] table:
			create table Meals ( 
				meal_id int auto_increment primary key,
				meal_name varchar(100) not null,
				cuisine varchar(50),
				cooking_time int not null default 1 check (cooking_time > 0),
				vegetarian boolean
				);

		- To create the [Ingredients] table:
			create table Ingredients ( 
				ingredient_id int auto_increment primary key,
				meal_id int not null,
				ingredient_name varchar(100) not null,
				quantity varchar(50),
				foreign key (meal_id) references Meals(meal_id) on delete cascade
				);

		- Finally check that the table is displaying properly for the main data points:
			show tables;

		- To check specifically the data that will show in each individual data sets, use:
			describe Ingredients;
			describe Meals;	


- Insert Data:
	- Now that the table and their headers are created, we will begin to add data to them. The first command adds four records to the [Meals] table:
	- The first command will add four record data points to the [Meals] table:
		insert into Meals (meal_name, cuisine, cooking_time, vegetarian) values
			('Spaghetti Bolognese', 'Italian', 45, FALSE),
			('Vegetable Stir Fry', 'Chinese', 20, TRUE),
			('Chicken Curry', 'Indian', 50, FALSE),		
			('Mushroom Risotto', 'Italian', 35, TRUE);

	- The second command adds the list of ingredients for the meals in that we listed in the [Meals] table. The first number is for the [meal_id], and that number will match the number produced in the [Meals] table, which can be seen by using:
		select * form Meals;
	- To add the [Ingredients] list to the table, use:
	
		insert into Ingredients (meal_id, ingredient_name, quantity) values
			(1, 'Spaghetti', '200g'),
			(1, 'Ground Beef', '250g'),
			(1, 'Tomato Sauce', '1 cup'),
			(2, 'Broccoli', '100g'),
			(2, 'Carrots', '50g'),
			(2, 'Soy Sauce', '2T'),
			(3, 'Chicken Breast', '300g'),
			(3, 'Curry Powder', '2T'),
			(3, 'Coconut Milk', '1 cup'),
			(4, 'Arborio Rice', '1 cup'),
			(4, 'Mushrooms', '1 cup'),
			(4, 'Parmesan Cheese', '1/2 cup');

- Querying Data:
	- Now that the tables are created and we added information to them, we can begin to query them. The following command is a simple [SELECT] statement that returns information about the [Meals] table:
		select * from Meals;	

	- We can filter the results with the [WHERE] clause. For this practice, we will filter by vegetarian:
		select * from Meals where vegetarian = TRUE;

	- We can sort the results by descending or ascending order. This works for both alphabetical data and numerical characters. Here will will sort by cooking_time:
		select * from Meals order by cooking_time desc;
		select * from Meals order by cooking_time asc;

	- Utilizing the following command, we select three values:
		- [meal_name] from the [Meals] table and rename the resulting column [Meals].
		- [ingredient_name] from the [Ingredients] table and rename the resulting column [Ingredients].
		- [quantity] from the [Ingredients] table and rename the resulting column [Quantity].

	- Using the [join] action to cross-reference the tables based on the shared [meal_id] value.
	 
		select Meals.meal_name as Meals,
			Ingredients.ingredient_name as Ingredients,
			Ingredients.quantity as Quantity 
			from Meals
			join Ingredients on Meals.meal_id = Ingredients.meal_id;

	- Here, we list the ingredients and their quantities based on the name of the meal. Specifically here, we are looking at Chicken Curry:
	
		select ingredient_name as Ingredients,
			quantity as Quantity
			from Ingredients
			where meal_id = (select meal_id from Meals where meal_name = 'Chicken Curry');

	- Here, we will provide a count of the Meals by cuisine:
	
		select cuisine, count(*) as meal_count
			from Meals
			group by cuisine;


	- Finally, if it's been a long Monday, and we don't want to be cooking forever, we cam select meals that take less than 45 minues:
	
		select meal_name, cooking_time
			from Meals
			where cooking_time <= 45
			order by cooking_time asc;



- Database Management
	- When starting this lesson, we created a database called [DinnerDB], and we granted [opacuser] all privileges to this database. 
	- If we wanted to revoke those privileges, we log into the root MYSQL user:
	
		sudo mysql -u root
		
	- To re-review the privilages for [opacuser]:
	
		mysql> show grants for 'opacuser'@'localhost';
		
	- We can take those privilages away with the [REVOKE] command:
	
		mysql> revoke all privilages on DinnerDB.* from 'opacuser'@'localhost';
		
	- To confirm, we can re-run the [show grants] command.
	
	- If we want to view other user accounts on the MySQL server, the following command queries the [user] table in the [mysql] database and will return all user accounts:
	
		select user, host from mysql.user;
		
	- We can also delete the database using the [DROP] command:
	
		mysql> drop database DinnerDB;
					


###Creating a Bare Bones OPAC###

- Before creating a search form, we will update our table so the [copyright] column uses a proper [date] data type. This will allow our filters run true start and end dates. 
	- To start, we will do the following: 
	
		mysql -u opacuser -p
		
		mysql> use opacdb;
		
		mysql> alter table books add publication_date date;
		
		mysql> update books set publication_date = str_to_date(concat(copyright, '-01-01'), '%Y-%m-%d');
		
		mysql> alter table books drop column copyright;
		
		mysql> alter table books change publication_date copyright date not null;
		
		mysql> \q

- Next,
	- To begin, we will run the following:
			cd /var/www/html/
			sudo micro mylibrary.html
	- Then add the HTML form:
		<!DOCTYPE html>
			<html>
			    <head>
			        <meta charset="UTF-8">
			        <title>MySQL Server Example</title>
			    </head>
			<body>
			
			    <h1>A Basic OPAC</h1>
			
			    <p>In the form below, <b>optionally</b> enter text in the search field.
			    Your search query will search by author, title, or publisher.
			    Capitalization is usually not necessary on default case-insensitive MySQL collations.
			    It's okay to enter partial information, like part of an author's, title's, or publisher's name.</p>
			
			    <p>You can leave the search field empty and only enter dates.
			    Regardless, both start and end dates are required for all searches.
			    You can use the date fields to limit results, too.
			    I added some extra records, which you can view to know what you can query:</p>
			
			    <p><a href="opac.php">OPAC</a></p>
			
			    <p>This is very much a toy, stripped down
			    <a href="https://en.wikipedia.org/wiki/Online_public_access_catalog">OPAC</a>.
			    The records are basic.
			    Not only do they not conform to <a href="https://www.loc.gov/marc/">MARC</a>,
			    they don't even conform to something as simple as <a href="https://www.dublincore.org/">Dublin Core</a>.</p>
			
			    <p>I also don't provide options to select different fields, like author, title, or publisher fields.
			    Instead the search field below searches key bibliographic fields (author, title, publisher) in our <b>books</b> table.</p>
			
			    <p>The key idea is to get a sense of how an OPAC works, though.</p>
			
			    <h2>My Basic Library OPAC</h2>
			
			    <form method="post" action="search.php">
			        <label for="search">Search Terms (optional):</label>
			        <input type="text" name="search" id="search">
			        
			        <br>
			        
			        <label for="start_date">Start Date:</label>
			        <input type="date" name="start_date" id="start_date" required>
			        
			        <br>
			        
			        <label for="end_date">End Date:</label>
			        <input type="date" name="end_date" id="end_date" required>
			        
			        <br>
			        
			        <input type="submit" value="Search">
			    </form>
			
			</body>
			</html>

	- To find your OPAC page, go to:
		<http://34.171.244.152/mylibrary.html>

	- Next we will save and close. Then go out of micro and use the following command:
		sudo micro search.php
	- Then add the PHP Search Script:
		<!DOCTYPE html>
		<html lang="en">
		<head>
		    <meta charset="UTF-8">
		    <meta name="viewport" content="width=device-width, initial-scale=1.0">
		    <title>Search Results</title>
		<style>
		    table {
		        border-collapse: collapse;
		        width: 100%;
		    }
		    th, td {
		        border: 1px solid black;
		        padding: 8px;
		        text-align: left;
		    }
		</style>
		</head>
		<body>
		
		    <h1>Search Results</h1>
		
		    <?php
		    // Load MySQL credentials
		    require_once '/var/www/login.php';
		
		    // Enable MySQL error reporting
		    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
		
		    // Establish connection
		    $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
		    if ($conn->connect_error) {
		        die("Connection failed: " . $conn->connect_error);
		    }
		
		    if ($_SERVER["REQUEST_METHOD"] == "POST") {
		        $search = trim($_POST['search']);
		        $start_date = $_POST['start_date'];
		        $end_date = $_POST['end_date'];
		
		        // Prepared statement to prevent SQL injection
		        $stmt = $conn->prepare("SELECT id, author, title, publisher, copyright FROM books 
		                                WHERE (author LIKE ? OR title LIKE ? OR publisher LIKE ?) 
		                                AND copyright BETWEEN ? AND ?");
		
		        // Use wildcard search
		        $search_param = "%$search%";
		        $stmt->bind_param("sssss", $search_param, $search_param, $search_param, $start_date, $end_date);
		        $stmt->execute();
		        $result = $stmt->get_result();
		
		        if ($result->num_rows > 0) {
		            echo "<table>";
		            echo "<tr><th>ID</th><th>Author</th><th>Title</th><th>Publisher</th><th>Copyright</th></tr>";
		
		            while ($row = $result->fetch_assoc()) {
		                echo "<tr>";
		                echo "<td>" . htmlspecialchars($row["id"]) . "</td>";
		                echo "<td>" . htmlspecialchars($row["author"]) . "</td>";
		                echo "<td>" . htmlspecialchars($row["title"]) . "</td>";
		                echo "<td>" . htmlspecialchars($row["publisher"]) . "</td>";
		                echo "<td>" . htmlspecialchars($row["copyright"]) . "</td>";
		                echo "</tr>";
		            }
		
		            echo "</table>";
		        } else {
		            echo "<p>No results found.</p>";
		        }
		
		        $stmt->close();
		    }
		
		    $conn->close();
		    ?>
		
		    <p><a href="mylibrary.html">Return to search page</a></p>
		
		</body>
		</html>

- Modifications:
	- 	Reconnect to MySQL
		mysql -u opacuser -p

	- Open the correct database:
		use opacdb;

	- Then run the [insert] command with the data for the new records:
		insert into books
		(author, title, publisher, copyright) values
		('Perry Strausbaugh', 'Flora of West Virginia', 'Seneca Books, Inc.', '1978-01-01');	
				

- Creating the HTML Page and a PHP Cataloging Page
	- First we will create a new directory:
		cd /var/www/html
		sudo mkdir cataloging

	- Then we will change to the catalonging directory:
		cd cataloging/

	- In that directory, open up your text editor:
		sudo micro index.html

	- In the index.html, we will add:
		<!DOCTYPE html>
		<html>
		<head>
		    <title>Enter Records</title>
		</head>
		<body>
		    <h1>OPAC Library Administration</h1>
		
		    <p>This is the library administration page for entering records into the OPAC.</p>
		    <p>Please do not use this page unless you are an authorized cataloger.</p>
		
		    <form action="insert.php" method="post">
		        <label for="author">Author:</label>
		        <input type="text" name="author" id="author" required><br><br>
		
		        <label for="title">Book Title:</label>
		        <input type="text" name="title" id="title" required><br><br>
		
		        <label for="publisher">Publisher:</label>
		        <input type="text" name="publisher" id="publisher" required><br><br>
		
		        <label for="copyright">Copyright:</label>
		        <input type="date" name="copyright" id="copyright" required>
		
		        <input type="submit" value="Submit">
		    </form>
		</body>
		</html>


- PHP Insert Script
	- The index.html page will provide a user interface, that is a form for entering bib data. We will need the PHP script to communicate and add the data from our form into our MySQL database and books table.

	  Additionally, we will want the PHP script to also match the form from the HTML page and the data structure in the books table. 
	- Next we will save and go out. Here we will type the following command:
		sudo micro insert.php
	- Then we will put the PHP script in, called insert.php:
		<!DOCTYPE html>
		<html>
		<head>
		    <meta charset="UTF-8">
		    <meta name="viewport" content="width=device-width, initial-scale=1.0">
		    <title>Cataloging: Data Entry</title>
		</head>
		<body>
		
		<h1>Cataloging: Data Entry</h1>
		
		<?php
		
		// Load MySQL credentials
		require_once '/var/www/login.php';
		
		// Enable MySQL error reporting
		mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
		
		// Establish connection
		$conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
		if ($conn->connect_error) {
		    die("Connection failed: " . $conn->connect_error);
		}
		
		if ($_SERVER["REQUEST_METHOD"] === "POST") {
		    $author = trim($_POST["author"] ?? "");
		    $title = trim($_POST["title"] ?? "");
		    $publisher = trim($_POST["publisher"] ?? "");
		    $copyright = $_POST["copyright"] ?? "";
		
		    if ($author === "" || $title === "" || $publisher === "" || $copyright === "") {
		        echo "All fields are required.";
		    } elseif (!preg_match('/^\d{4}-\d{2}-\d{2}$/', $copyright)) {
		        echo "Copyright date must use YYYY-MM-DD format.";
		    } else {
		        // Prepare and bind SQL statement
		        $stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
		        $stmt->bind_param("ssss", $author, $title, $publisher, $copyright);
		
		        if ($stmt->execute() === TRUE) {
		            echo "New record created successfully";
		        } else {
		            echo "Error: " . $stmt->error;
		        }
		        $stmt->close();
		    }
		} else {
		    echo "Please submit records using the cataloging form.";
		}
		
		// Close connection
		$conn->close();
		?>
		
		<p><a href='index.html'>Return to Cataloging Page</a></p>
		<p><a href='../mylibrary.html'>Return to Library Home Page</a></p>
		</body>
		</html>
		
		
		
- Security
	- Our HTML and PHP files allow us to enter data into our MySQL database from a simple web interface, we want to limit access to the module. 
		In the real world, there would be more security (like DUO). For ours, we will rely on a simple  authorization mechanism provided by the Apache2 server called htpasswd. 

	- Our first step is to create an authentication file in our /etc/apache2 directory, which is where the Apache2 web server stores its configuration files. 
		sudo htpasswd -c /etc/apache2/.htpasswd libcat
		
			- Note: Use the -c option only the first time to create the file. To add additional users later, omit -c so existing accounts are not overwritten.

		- Then create the password:
			libcat123
		- To check the hash password, use:
				cat /etc/apache2/.htpasswd

	- Our next step is to tell the Apache2 web server that we will ise the [htpasswd] to control access to our cataloging module. 

	  To do that, we will use the text editor to open the apache2.conf file:
	  
	  	sudo nano /etc/apache2/apache2.conf

	  	- Scroll down to the <Directory /var/www/> block, and paste the following directly below it:
	  	
	  		<Directory /var/www/html/cataloging/>
	  		
	  		  Options Indexes FollowSymLinks
	  		  
	  		  AllowOverride AuthConfig
	  		  
	  		  Require all granted
	  		  
	  		</Directory>

	  - Then, we will go to the cataloging directory and use our text editor to create a file called .htaccess:
	  
	  	sudo micro .htaccess

	  - Then add the following information: 
	  
	  			AuthType Basic
	  			
	  			AuthName "Authorization Required"
	  			
	  			AuthUserFile /etc/apache2/.htpasswd
	  			
	  			Require valid-user	

	- Next, we will check if the file is okay:
	
		sudo apachectl configtest

	- If it says "Syntax OK" restart Apache2 and check its status:
	
		sudo systemctl restart apache2
		sudo systemctl status apache2


- Permissions and Ownership
	- Apache2 web server has a user account on your Linux server. The account name is www-data, and it's account details are stored in the /etc/passwd file:


	- To initiate these guidelines with the chown and chmod commands:
		sudo chown :www-data /var/www/html
	- Set the setgid bit on /var/www/html.
		sudo find /var/www/html -type d -exec chmod g+s {} +





- Get Cataloging!
	- Now its time to visit your cataloging module in a web browser using your server's IP address:
		- http://34.171.244.152/cataloging/index.html

	- You can also test the PHP page directly:
		- http://34.171.244.152/cataloging/insert.php

	- In both cases, Apache2 should require the username and password that you created with htpasswd.




- Results: We now have a working OPAC, with a password for set for security. 
- Verification: To get to my Catalog: [Sienna's Catalog](http://34.171.244.152/cataloging/index.html) 
- Notes: 
	- One big achievement was figuring out how to delete a column that I messed up in the previous module. It took some diggining but I found to do that, you use the following:
		- alter table books drop column published;
		* Note, which ended up being covered in the second video.
	- At some point while creating the bare bones OPAC, I messed something up, and had to recreate my [opacdb], which wasn't as bad of a process than I thought. 


- Reflections:

		- o	Overall, the OPAC and cataloging module chapters in the textbook were clear. I think as we worked on building our barebones OPAC, things began to make a lot more sense. Just reading the information at first, things did not click, but as we moved through creating our relational databases, creating the HTML page and PHP search page, and creating the cataloging page, everything made a lot more sense. In this course, I have found that the visual examples and working through these exercises have helped me the most. 

		- o	Overall, most of this process was easy to follow. I made sure to consistently take my time (until the end where I made a silly mistake that I will discuss below). I did do a little extra reading in understanding the connection between all of these systems to better understand how they are all connected and what they are doing.

		- o	Attention to detail is critical in working with databases, software code, and system documentation because each error can cause huge issues either immediately, or down the road. One issue I ran into, which demonstrates why attention to detail is important is near the end, when I was telling Apache2 web server that the webpage was going to have control access. I didn’t realize that I mistyped the command [sudo micro /etc/apache2/apache2.conf]. I had mistyped the ‘etc’ and typed “ect”, which was a small error that caused an internal error on my website. 

		- o	An issue that could occur down the road is if we are not working carefully in our databases. If we are not careful when typing in information, then our relational databases will not connect to each other.

		- o	This assignment connects to real-world library systems and their maintenance, because even though this is a bare bones version, this is still a building block into what our databases are today. As we move forward, we can build upon this foundational knowledge to propel us forward into the more complex versions of the databases we use. 
		

