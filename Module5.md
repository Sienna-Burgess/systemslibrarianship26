# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026
### Module 5: DIY Integrayed Library Systems
---
This page serves to document my work in LIS 624 via a Github repository. This will document my work from week 4 going forward. 
As time goes on in this course, this description will change and include additional information. 

---

 **Module 10: Creating a Barebines OPAC and Cateloging Module**
- Goal: To practice basic MySQL commands to better understand how relational databases are structured, queried, and managed. 
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
					






- Results:
- Verification: *** Add GitHub Module 5 link?
- Notes: **Reflect here!!!!!!

