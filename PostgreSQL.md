# PostgreSQL

## Table of Contents
[Linux Distribution](#linux-distribution)

[Prerequisite tool](#prerequisite-tool)

[Podman](#podman)

[PostgreSQL](#postgresql)

[PostgreSQL setup](#setup-postgresql)

[Table](#table)

[Keyword](#keyword)

[Operator](#operator)

[Inbuild function](#inbuild-function)

[User defined function](#user-defined-function)

[String](#string)

[Reference link](#reference-link)

## Linux Distribution:

Distributor Id - Ubuntu

Version - 20.04

## Prerequisite tool:

Podman (version 3.4.2)



## Podman:
Podman is an open-source container management tool that allows users to manage containers without the need for a container daemon. It is designed to be a lightweight, daemonless alternative to Docker. Podman provides a command-line interface (CLI) for managing containers, pods, and container images.

**Update your system:-**
```
sudo apt update
```
**Podman Install:-**
```
sudo apt install podman
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ sudo apt install podman
[sudo] password for vivek:
Reading package lists... Done
Building dependency tree   
Reading state information... Done
podman is already the newest version (100:3.4.2-5).
0 upgraded, 0 newly installed, 0 to remove and 9 not upgraded.
```
**- sudo:** This part of the command is like saying "I want to do something important." It
stands for "superuser do" and allows you to perform tasks that affect your computer's
system, like installing software.

**- apt:** It is used to handle the installation, removal, and upgrading of software packages.

 **install:** This tells the magic tool that you want to put a new program on your
computer.


**Check podman :-**
```
which podman
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ which podman
/usr/bin/podman 
vivek@vivek-HP-EliteBook-840-G2:~$
```
**Which**: Which command is used to locate the full path of the executable file.

## PostgreSQL:
**Definition:-**  PostgreSQL is an open source relational database management system  that performs a variety of tasks related to storing, organising, and retrieving structured data. It is an advanced Enterprise class. It supports SQL (relational) and json (non-relational).

**Features:-** Table inheritance user-define type , foreign key view, rule, subquery, asynchronous, replication(fast of db).

## Setup PostgreSQL:

**Search the Postgres Image in podman**:
```
podman search postgres
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman search postgres
INDEX       NAME                                                 DESCRIPTION                                      STARS       OFFICIAL    AUTOMATED
docker.io   docker.io/library/postgres                           The PostgreSQL object-relational database sy...  12889       [OK]       
```
**podman**: This is a container management tool.

**search**: This subcommand is used to search for container images available in the container registry.

**postgres**: This is the search keyword indicating the container image you are looking for. 

**copy the image name and paste it when run the container**:

**run the container of postgres**:
```
podman run -d -p 5432:5432 -v /home/vivek/postgres/data:/var/lib/pgsql/data --name postgres -e POSTGRES_PASSWORD=your_password docker.io/library/postgres:latest
```
**Podman run :** This command is used to run a container using podman.

`-d: `The -d flag runs the container in the background (detached mode).

`-P 5432:`This option maps port 5432 from the host machine to port 5432 in the container.

**-v /home/vivek/postgres/data:/var/lib/pgsql/data:** This option mounts a volume, linking the /home/vivek/postgres/data directory on your host machine to the /var/lib/pgsql/data directory inside the container. This allows data to persist even if the container is stopped or removed.

**--name postgres:** This assigns the name "postgres" to the running container.

**-e POSTGRES_PASSWORD=your_password:** This sets an environment variable POSTGRES_PASSWORD within the container, specifying the password.

**docker.io/library/postgres:latest:** This is the image name for the PostgreSQL container. It tells Podman which container image to use. The image is pulled from the Docker Hub.

Check running container of postgres-
```
podman ps
```
**Output**:
```
vivek@vivek-HP-EliteBook-840-G2:~$ podman ps

CONTAINER ID  IMAGE                          COMMAND CREATED STATUS        PORTS               NAMES

48ef2005155e  docker.io/library/postgres:latest  postgres 4 days ago  Up 4 seconds ago  0.0.0.0:5432->5432/tcp  postgres
```
Now go to postgres container-
```
podman exec -it postgres /bin/bash
```
**Output**:
```
vivek\@vivek-HP-EliteBook-840-G2:\~$ podman exec -it postgres bash
root\@48ef2005155e:/#
```
**Podman exec :** This command for executing  within a running container.

**it:** It is commonly used for running an interactive shell inside the container.

**Container name:** This is a place for the name or ID of the container.

**/bin/bash:** This command will be executed inside the specified container.

Now check the Postgres version-
```
 psql –version
```
**Output**:
```
root@48ef2005155e:/# psql --version
psql (PostgreSQL) 16.1 (Debian 16.1-1.pgdg120+1)
root\@48ef2005155e:/#
```
**psql:** This is the PostgreSQL interactive terminal, a command-line tool that allows you to interact with PostgreSQL databases.

**--version:** This option is used to display the version information for the psql command. When you execute psql --version, the command returns information about the installed version of the PostgreSQL client.

To connect to a PostgreSQL database with a specific user-
```
psql -U postgres
```
**Output**:
```
root@48ef2005155e:/# psql -U postgres
psql (16.1 (Debian 16.1-1.pgdg120+1))
Type "help" for help.
postgres=#
```
**psql:** This is the PostgreSQL interactive terminal, a command-line tool that allows you to interact with PostgreSQL databases.

**-U postgres:** This option specifies the PostgreSQL username to use when connecting to the database.

To see available list of database -
```
\l
```
**Output**:
```
postgres=# \l
                                           List of databases
   Name |  Owner   | Encoding | Locale Provider |  Collate   |   Ctype | ICU Locale | ICU Rules |   Access privileges   

\-----------+----------+----------+-----------------+------------+------------+------------+-----------+--------------

 postgres  | postgres | UTF8 | libc        | en\_US.utf8 | en\_US.utf8 |        |       |
 template0 | postgres | UTF8 | libc        | en\_US.utf8 | en\_US.utf8 |        |       | =c/postgres      +
        |      |      |             |        |        |        |       | postgres=CTc/postgres
 template1 | postgres | UTF8 | libc        | en\_US.utf8 | en\_US.utf8 |        |       | =c/postgres      +
        |      |      |             |        |        |        |       | postgres=CTc/postgres
(4 rows)
```


**\ :** The backslash (\) commands are specific to the psql command-line tool and provide a convenient way to interact with the PostgreSQL database.

**l :** This is a shorthand for the "list" command in psql.

**Database:**  A database is like a digital filing cabinet where you can store and organise information. It's a structured collection of data that can be easily accessed, managed, and updated.

Now we create database -
```
create database <db name>;
```
**Output**:
```
postgres=# create database keenable;
CREATE DATABASE
postgres=#
```
**CREATE DATABASE:** This is the SQL command that instructs PostgreSQL to create a new database.

**keenable:** This is the name of the new database.

**; :** The semicolon (;) is used in SQL, including PostgreSQL, as a statement terminator. It signifies the end of an SQL statement and is required to execute the statement.

To Switch other database-
```
\c <database name>
```
**Output**:
```
postgres=# \c keenable;
You are now connected to database "keenable" as user "postgres".
keenable=#
```
**\c:** This is the shorthand for the "connect" command in psql.
When you enter \c followed by a database name, it prompts you to connect to a different database.

If you want to exit the database-
```
\q
```
**Output**:
```
keenable=# \q
root@48ef2005155e:/#
```
**\q:** This is a shorthand for the "quit" command in psql.
It is used to exit the PostgreSQL interactive terminal (psql) and return to the regular command prompt or shell.

**Schema:** A schema is a logical way to organise and structure data.  It defines the blueprint for how data is stored, organised, and related to one another in a database system. A schema includes the specifications for tables, columns, data types, relationships, and constraints, providing a framework for the database's structure.

Now create a schema-
```
create schema <schema_name>;
```
**Output**:
```
keenable=# create schema new;
CREATE SCHEMA
keenable=#
```
## Table:
A table in PostgreSQL is like a spreadsheet where information is organized into rows and columns.Each row represents a specific thing, like a customer or product, and each column holds a particular piece of information, such as a name or price. Tables help keep data organised, making it easier to find and manage.

Now create a table-
```
create table intern (id int not null, fname text not null, lname text not null, joining\_date int not null, month text not null, age int not null, city text not null);
```
**Output**:
```
keenable=# create table intern (id int not null, fname text not null, lname text not null, joining\_date int not null, month text not null, age int not null, city text not null);
CREATE TABLE
keenable=#
```
**create table intern:**  This part starts the creation of a new table named "intern."

**id int not null:** This line defines a column named "id" with the data type "int" (integer). The "not null" constraint means that this column must always have a value.

**fname text not null,:** This line creates a column named "fname" with the data type "text" (variable-length character). 

**lname text not null,:** Similar to "fname," this line creates a column named "lname" for the last name.

**joining\_date int not null,:** This line creates a column named "joining\_date" of type "int" for the date the intern joined.

**month text not null,:** This line adds a column for the month of joining, using the "text" data type.

**age int not null,:** Creates a column for the age of the intern using the "int" data type.

**city text not null:** Create a column for the city of residence, using the "text" data type.

To check list all tables in the current database:
```
\d
```
**Output**:
```
keenable=# \d
      List of relations
 Schema |  Name  | Type  |  Owner   
--------+--------+-------+----------
 public | intern | table | postgres
(1 row)
```
**\d:**This command is used to list various types of objects in the current database.

To list tables with additional details (columns, types, constraints):
```
\d tablename
```
**Output**:
```
keenable=# \d intern
               Table "public.intern"
Column |  Type   | Collation | Nullable | Default
--------------+---------+-----------+----------+---------
 id           | integer |    | not null |
 fname        | text |       | not null |
 lname        | text |       | not null |
 Joining_date | integer |    | not null |
 month        | text |       | not null |
 age          | integer |    | not null |
 city         | text |       | not null |
keenable=#
```
Insert value into table:
```
insert into intern values (101, 'manish', 'burman', 6, 'september', 21, 'delhi');
```
**Output**:
```
keenable=# insert into intern values (101, 'manish', 'burman', 6, 'september', 21, 'delhi');
INSERT 0 1
keenable=#
```
 **Insert into:** This statement is used in SQL to add new records (rows) into a table.

 **Intern:** Intern is the name of the table.

**Values:** It indicates that you are about to provide the values for the columns.


**Update data in table:** To update existing data in a table, you use the update statement.
```
update intern set fname= 'raghav' where fname ='manish';
```
**Output**:
```
keenable=# update intern set fname= 'raghav' where fname ='manish';
UPDATE 1
 101 | raghav   | burman |        6 | september |  21 | delhi |
```

UPDATE: This keyword indicates that you want to modify data in a table.

intern: This is the name of the table you want to update.

SET: This keyword is used to specify the column(s) and their new values that you want to update.

fname: This is the column you want to update.

'raghav': This is the new value you want to set for the "fname" column.

WHERE: This clause is used to specify a condition to determine which rows to update. In this case, you want to update the rows where the value in the "fname" column is equal to 'manish'.

fname = 'manish': This is the condition. It ensures that only rows where the "fname" column has the value 'manish' will be updated.

**Delete data from table:** To delete data from a table, you use the DELETE statement.
```
delete from intern where fname = 'raghav';
```
**Output**:
```
keenable=# delete from intern where fname = 'raghav';
DELETE 1
```
DELETE FROM: This phrase is used to specify that you want to delete records from a table.

intern: This is the name of the table from which you want to delete records.

WHERE: This clause is optional but, if used, it specifies a condition to determine which rows to delete. In this case, you want to delete the rows where the value in the "fname" column is equal to 'raghav'.

fname = 'raghav': This is the condition. It ensures that only rows where the "fname" column has the value 'raghav' will be deleted.

**Add a column to a table:** To add a new column to an existing table, you use the ALTER TABLE statement.
```
Alter table contractor add column email text;
```
**Output**:
```
keenable=# alter table contractor add column email text;
ALTER TABLE
keenable=# select * from contractor;
 id |  name  | salary |        work_hour         | email
----+--------+---------------+----------------------------------+-------
```
ALTER TABLE: This phrase is used to modify an existing table structure.

contractor: This is the name of the table to which you want to make changes.

ADD COLUMN: This specifies that you want to add a new column to the table.

email: This is the name of the new column that you want to add.

text: This specifies the data type for the new column. In this case, the data type is "text," which is often used for storing variable-length character strings.

**Delete a column from a table:** To delete an existing column from a table, you use the ALTER TABLE statement.
```
Alter table contractor drop column email;
```
**Output**:
```
keenable=#Alter table  contractor drop column email;
ALTER TABLE
```

ALTER TABLE: This phrase is used to modify an existing table structure.

contractor: This is the name of the table from which you want to remove a column.

DROP COLUMN: This specifies that you want to remove a column from the table.

email: This is the name of the column that you want to drop.

**Add a row to a table:** To add a new row (record) to a table, you use the INSERT INTO statement.
```
insert into intern values (106, 'kanha', 'kumar', 14, 'august', 28, 'bihar');
```
**Output**:
```
keenable=# insert into intern values (106, 'kanha', 'kumar', 14, 'august', 28, 'bihar');
INSERT 0 1
```

```
Delete from intern where fname = 'kanha';
```
```
keenable=# Delete from  intern where fname = 'kanha';
DELETE 1
```
**Rename column:** To rename a column, use the ALTER TABLE statement with the RENAME COLUMN clause.
```
Alter table contractor rename column morning to evening;
```
**Output**:
```
keenable=#Alter table contractor rename column morning to evening;
ALTER TABLE
```
**Rename table:** To rename a table, use the ALTER TABLE statement with the RENAME TO clause.
```
ALTER TABLE intern RENAME TO interns;
```
**Output**:
```
keenable=# ALTER TABLE intern RENAME TO interns;
ALTER TABLE
```
## keyword:
A "keyword" in Postgres is a reserved term used to represent specific operations in SQL queries, such as SELECT, INSERT, UPDATE, DELETE, etc

**Select keyword**:
```
select * from <table_name>;
```
**Output**:
```
keenable=# select * from intern;
 id  | fname  | lname  | joining_date |   month   | age | city  
-----+--------+--------+--------------+-----------+-----+-------
 101 | manish | burman |        6 | september |  21 | delhi
(1 row)
```
**SELECT:** This keyword is used to retrieve or fetch data from a database. Select data according to your requirement.

***:** The asterisk (*) represents all columns in the table. It essentially means "select everything."

**FROM <table_name>**: This part specifies the table from which you want to retrieve the data. Replace \<table\_name> with the actual name of the table.

**Clause**: A clause is a part of a SQL statement that specifies conditions for the retrieval or manipulation of data. Clauses are used to filter, sort the results of a query.

**WHERE Clause**: It is used to provide conditions in SELECT, UPDATE, and DELETE statements to filter rows based on a specified condition.
```
select * from intern where age>=20;
```
**Output**:
```
keenable=# select * from intern where age>=20;
 id  | fname  | lname  | joining_date |   month   | age | city  
-----+--------+--------+--------------+-----------+-----+-------
 101 | manish | burman |        6 | september |  21 | delhi
```
FROM intern: This specifies the table from which you want to retrieve data, in this case, the "intern" table.

WHERE age >= 20: This is a condition specified in the WHERE clause. It filters the rows, allowing only those where the value in the "age" column is greater than or equal to 20 to be included in the result set

**ORDER BY Clause:** It is used in SELECT statements to sort the result set based on one or more columns. Like- asc desc.
```
select * from intern order by age asc;
```
**Output**:
```
keenable=# select * from intern order by age asc;
 id  |  fname   | lname  | joining_date |   month   | age |   city    
-----+----------+--------+--------------+-----------+-----+-----------
 101 | manish   | burman |        6 | september |  21 | delhi
 105 | manoj | kumar  |       12 | july  |  23 | ghaziabad
 105 | vivek | pandey |       17 | september |  23 | kanpur
 105 | kanhaiya | mishra |       17 | august |  27 | greater
```
ORDER BY age ASC: This clause is used to sort the result set based on the specified column. In this case, it orders the result set by the "age" column in ascending order (ASC stands for ascending). If you wanted descending order, you could use DESC instead of ASC

**GROUP BY Clause:** It is used to change a particular field or data in group and show the other table form.
```
Select name, sum(id) from intern group by name;
```
**HAVING Clause**: It is used to provide a condition and SELECT statements with GROUP BY to filter grouped rows based on a condition.
```
select  department_id, AVG(salary) from employees GROUP BY department_id HAVING AVG(salary) > 50000;
```
## Operator: 
Operators are symbols that used to perform operations on one or more expressions or values, and they play a crucial role in constructing SQL queries.Like  

**Arithmetic Operators**:

+ (addition)

- (subtraction)

* (multiplication)

/ (division)


**Logical Operators**:

AND (logical AND)

OR (logical OR)

NOT (logical NOT)

**Pattern Matching Operators**:

LIKE (pattern matching with wildcards)


**String Concatenation Operator:**

|| (concatenates two strings)

**AND**: The AND operator is used to combine multiple conditions in a SQL statement.
It returns true only if all the conditions joined by AND are true.
```
select * from intern where id =105 AND age>23;
```
**Output**:
```
keenable=# select * from intern where id =105 AND age>23;
 id  |  fname   | lname  | joining_date | month  | age |  city   
-----+----------+--------+--------------+--------+-----+---------
 105 | kanhaiya | mishra |       17 | august |  27 | greater
```
**OR:** The OR operator is used to combine multiple conditions in a SQL statement.
It returns true if at least one of the conditions joined by OR is true.
```
select * from intern where age>23 or id=106;
```
**Output**:
```
keenable=# select * from intern where age>23 or id=106;
 id  |  fname   | lname  | joining_date | month  | age |  city   
-----+----------+--------+--------------+--------+-----+---------
 105 | kanhaiya | mishra |       17 | august |  27 | greater
```
**NOT:** The `NOT` operator is used to negate a condition in a SQL statement.
   - It returns true if the condition following `NOT` is false and vice versa.

**LIKE :** The LIKE operator is used to match patterns in string data using wildcard characters.The % symbol is used to represent zero or more characters, and the _ (underscore) represents a single character.
```
SELECT * FROM intern WHERE fname LIKE 'k%';
```
```
keenable=# SELECT * FROM intern
WHERE fname LIKE 'k%';
 id  |  fname   | lname  | joining_date | month  | age |  city   
-----+----------+--------+--------------+--------+-----+---------
 105 | kanhaiya | mishra |       17 | august |  27 | greater
```
**IN:** The IN operator checks if a specified value matches any value in a list.
```
select * from intern where age in (19,21,22);
```
**Output**:
```
keenable=# select * from intern where age in (19,21,22);
 id  | fname  | lname  | joining_date |   month   | age | city  
-----+--------+--------+--------------+-----------+-----+-------
 101 | manish | burman |        6 | september |  21 | delhi
```
**NOT IN:** The NOT IN operator, as the name suggests, negates the condition of the IN operator. It checks if a specified value does not match any value in a list.
```
select * from intern where age not in(23,23,27);
```
**Output**:
```
keenable=# select * from intern where age not in(23,23,27);
 id  | fname  | lname  | joining_date |   month   | age | city  
-----+--------+--------+--------------+-----------+-----+-------
 101 | manish | burman |        6 | september |  21 | delhi
```
**BETWEEN:** The BETWEEN operator in PostgreSQL is used to filter rows based on a range of values. It is often used in the WHERE clause of a SQL statement to retrieve rows where a column's value falls within a specified range.
```
select * from intern where age between 21 and  23;
```
**Output**:
```
keenable=# select * from intern where age between 21 and  23;
 id  | fname  | lname  | joining_date |   month   | age |   city    
-----+--------+--------+--------------+-----------+-----+-----------
 101 | manish | burman |        6 | september |  21 | delhi
 105 | manoj  | kumar  |       12 | july  |  23 | ghaziabad
 105 | vivek  | pandey |       17 | september |  23 | kanpur
```
**View:** A view is a virtual table that is based on the result of a SELECT query. It does not store the data itself but provides a way to represent the result of a query as if it were a table.
```
Create view keen(view name) as select fname age from intern;
```
**Inner Join:** A JOIN in PostgreSQL is used to combine rows from two or more tables based on a related column between them.
```
Select intern.fname, intern.name, contractor.name from intern inner join contractor.on intern.id=contactor.id;
```
**Left or left outer join:** A LEFT JOIN returns all rows from the left table and the matched rows from the right table. If there is no match, NULL values are returned for columns from the right table.
```
Select intern.fname, intern.age, contractor name from intern left  join contractor on intern.id=contractor.id;
```
**Right or right outer join:** A RIGHT JOIN is similar to a LEFT JOIN, but it returns all rows from the right table and the matched rows from the left table.
```
Select  intern.fname, intern.age, contractor name from intern right  join contractor on intern.id=contractor.id;
```
**Full outer join:** A FULL OUTER JOIN returns all rows when there is a match in either the left or the right table. If there is no match, NULL values are returned for columns from the table without a match.
```
Select intern.fname, intern.age, contractor.name from contractor full outer join intern on intern.id=contractor.id;
```
**Cross join:** don't provide conditions if two tables are available  and each of them table 4 row then 4x4=16 rows will be created.
```
Select fname, name from intern cross join contractor;
```
## Inbuild function:
In PostgreSQL, an "inbuilt function" refers to a pre-defined operation that you can use in your SQL queries to perform specific tasks. These functions are already built into the database system.

**Sum**: Adds up all the values in a numeric column.
```
Select sum(salary) from table_name;
```
**Output**:
```
keenable=# select sum(salary) as salary from employee;
 salary
--------
 155000
(1 row)
```
**Avg:** Calculates the average (mean) of values in a numeric column.
```
Select avg(age) from intern;
```
**Output**:
```
keenable=# select avg(age) from intern;
      avg     
---------------------
 23.500
```
**Max:**  Finds the highest value in a column.
```
Select max(salary) from employee;
```
**Otput**:
```
keenable=# select max(salary) from employee;
  max  
-------
 50000
```
**Min:** Finds the lowest value in a column.
```
Select min(salary) from employee;
```
**Output**:
```
keenable=# select min(salary) from employee;
  min  
-------
 25000
```
**Count:** Counts the number of rows in a result set.
```
select count(age) from intern;
```
**Output**:
```
keenable=# SELECT COUNT(age) FROM intern;
 count
-------
 4
```
## User defined function:
A user-defined function (UDF) is like a custom operation or calculation that you create in a database to perform a specific task.

**Triggers:** Triggers are a set of action database call back functions that run  automatically when a specified database event is performed on a specified table.
Event- update, delete, select. 

**Alias:** Alias is like a nickname or a short name that you give to a column or a table in a query. It helps make your query results more readable and provides a way to reference columns or tables with a different name.
```
Select salary as total from employee;
```
**Output**:
```
keenable=# SELECT fname AS "First", lname AS "Last"
FROM intern;
  First   |  Last  
----------+--------
 manish   | burman
 manoj    | kumar
 kanhaiya | mishra
 vivek    | pandey
```
SELECT salary AS total: This part of the query selects the "salary" column from the "employee" table and aliases it as "total" in the result set.

**Index:** index is a database object that improves the speed of data retrieval operations on a table.
```
Create index intern_index on intern(“fname”);
```
**Array:** Array is like a collection or a list that holds multiple items of the same type. It allows you to group related pieces of data together under a single name.
```
Create table contractor (id int not null primary key, name text not null, salary int [ ] work_hour text [ ] [ ]);
```
If you want to access the value then run these command-
```
1-D array- select salary [ ] from table_name;
```
```
2-D array- select work_hour[1][1] from contractor;
```
**Enumeration:** Using this function we can create our according data type. 


```
Create type mood as enum (‘sad’,’ok’,’happy’);
```
**Output**:
```
keenable=# create type mood as enum('sad','ok','happy');
CREATE TYPE
```
CREATE TYPE mood: This part of the statement indicates that you are creating a new user-defined data type named "mood."

AS ENUM ('sad', 'ok', 'happy'): This specifies the values that the "mood" type can take. In this case, it is an enumeration type, and the possible values are 'sad', 'ok', and 'happy'. Enumerations are used to define a set of distinct values that a column can take.

**Constraint:** constraint is a rule or condition applied to a column or a set of columns in a table to maintain the integrity, accuracy, and reliability of the data. Constraints define the limitations or rules of the data in a table.

**Null constraint:** Ensures that a column does not contain any null (missing or undefined) values.Like- create table work (id not null);

**Check constraint:** Defines a condition that must be true for each row in a table.Like-
Roll no must be greater than 1000 in the table. If we insert roll_no in table less than 1000 then print error.
```
Create table rough (roll_no int check(roll_no > 1000), name text not null);
```
**Output**:
```
keenable=# Create table rough (roll_no int check(roll_no > 1000), name text not null);
Create table
keenable=# insert into rough values (990,'ramu');
ERROR:  new row for relation "rough" violates check constraint "rough_roll_no_check"
DETAIL:  Failing row contains (990, ramu).
```
## String: 
String functions are operations or methods that perform various tasks on strings (text data).

**Length:** Returns the number of characters in a string.
```
SELECT LENGTH('Hello') AS length;
```
**Output**:
```
keenable=# SELECT LENGTH('Hello') AS length;

 length
--------
   5
```
**Lower:** Converts a string to lowercase.
```
SELECT LOWER('Hello') AS lowercase;
```
**Output**:
```
keenable=# SELECT LOWER('Hello') AS lowercase;
 lowercase
-----------
 Hello
```
**Upper:** Converts a string to uppercase.
```
SELECT UPPER('world') AS uppercase;
```
**Output**:
```
keenable=# SELECT UPPER('world') AS uppercase;
 uppercase
-----------
 WORLD
```
**SUbstr:** Extracts a substring from a string based on a specified starting position and length.
```
SELECT SUBSTRING('Hello World' FROM 7 FOR 5) AS extracted_part;
```
**Output**:
```
keenable=# SELECT SUBSTRING('Hello World' FROM 7 FOR 5) AS extracted_part;
 extracted_part
----------------
 World
```
**Position:** Returns the position of a specified substring within a string**.**
```
SELECT POSITION('lo' IN 'Hello') AS position;
```
**Output**:
```
keenable=# SELECT POSITION('lo' IN 'Hello') AS position;
 position
----------
     4
```
**Ascii:** Returns the ASCII value of the first character in a string.
```
SELECT ASCII('A') AS ascii_value;
```
**Output**:
```
keenable=# SELECT ASCII('A') AS ascii_value;
 ascii_value
-------------
       65
```
**Reverse:** Reverses the order of characters in a string.
```
SELECT REVERSE('kumar') AS reversed_string;
```
**Output**:
```
keenable=# SELECT REVERSE('kumar') AS reversed_string;
 reversed_string
-----------------
 ramuk
```
**Repeat:** Repeats a string a specified number of times.
```
SELECT REPEAT('vivek', 3) AS repeated_string;
```
**Output**:
```
keenable=# SELECT REPEAT('vivek', 3) AS repeated_string;
 repeated_string
-----------------
 Vivekvivekvivek
```
**Concate:**  The CONCAT function in PostgreSQL is used to concatenate (join together) two or more strings into a single string. Additionally, you can also use the || operator for concatenation.
```
SELECT CONCAT('Hello ', 'World') AS concatenated_string;
```
**Output**:
```
 SELECT CONCAT('Hello ', 'World') AS concatenated_string;
 concatenated_string
---------------------
 Hello World
```
```
SELECT 'Hello ' || 'World' AS concatenated_string;
```
```
keenable=# SELECT 'Hello ' || 'World' AS concatenated_string;
 concatenated_string
---------------------
 Hello World
```
# Reference link
https://youtube.com/playlist?list=PLzAy3QBHoWZdxPXkD7UVymWm_Do3IdzwQ&si=ANT8eCKmB157ng9U

