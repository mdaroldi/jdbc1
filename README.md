# Java + Docker + MySQL + MacOS
Tutorial on how to access and use a Docker MySQL container from a Java program on Mac

----------------
#### Dependencies:
* Java
* Docker
* Eclipse, or another IDE

----------------

## Steps
	
* Download and install [MySQL Java Connector](https://dev.mysql.com/downloads/connector/j/5.1.html)
* Create an User Library using the .jar file
* Download a MySQL Docker image: 
`shell> docker pull mysql/mysql-server`

* Start running a docker container from MySQL image:
* 
`shell> docker run --name=mysql1 --env MYSQL_ROOT_HOST=% -p 3306:3306 -d mysql/mysql-server`

* Check the automatically generated password of root user, copy it

`shell> docker logs mysql1 2>&1 | grep GENERATED` 
* Connecting to the MySQL Server instance, and then paste the generated password:

`shell> docker exec -it mysql1 mysql -u root -p` 

The system will ask you to change to root password, use the following command to achieve it:

`mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';`

Obs.: This will change the generated password by 'password'. You may use any password, just don't forget to update it in db.properties file.

* In the client terminal, create the database 'coursejdbc' (if you prefer another name, update it in the db.properties file):

`mysql> CREATE DATABASE coursejdbc;`

* Then, copy the database.sql file to the container in a fresh terminal:

`$ docker cp /path/to/database.sql mysql1:/database.sql`

This may put the file in the root directory. In the mysql client terminal, you may check if it worked typing:

`mysql> system ls`

* Select this database:

`mysql> USE coursejdbc;`

* And finally, import the SQL commands from the file:

`mysql> source database.sql`

* You can check if everything is OK with this command:

`SELECT * FROM department;`

It might return a table with two columns - Id and Name.

* Now, you may run the Java code and get the following result:

>	1, Computers

>	2, Electronics

>	3, Fashion

>	4, Books

----------------
## Problem solving for remotely access
If you got the same problem like this while connect to MySQL server from another host (It depends on which version of MySQL you are using):

`java.sql.SQLNonTransientConnectionException: Public Key Retrieval is not allowed`

You should change your password of root user by using the native password hashing method to fix it:

`ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`