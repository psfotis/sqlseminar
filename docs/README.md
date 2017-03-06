# SQL SEMINAR
INTRODUCTION TO SQL SEMINAR



About

SQL is a powerful query language that provides the necessary constructs for data definition and manipulation with a goal set to empower a variety of real-world applications with data-centric tasks. In this seminar, participants will go through the basic SQL constructs and will learn how to construct and execute SQL queries against relation database management systems.


Main takeaways

* Background on the relation model and relational database management systems
* Creating and modifying relations using SQL
* Basic SELECT-FROM-WHERE statements
    * Math operations and constant expressions in the SELECT clause
    * Boolean and complex conditions in the WHERE clause
    * Pattern matching in the WHERE clause
* Cross products and join operations
* Aggregation and Grouping
    * Standard aggregation functions
    * GROUP BY and HAVING clauses
* UNION, INTERSECT, and EXCEPT operations
* The ORDER BY clause
* NULL values
* Nested queries

Preparation

The seminar includes a hands-on session to get participants familiar with querying a relational database using SQL. Participants will need to install before the seminar

* the PostgreSQL database (https://www.postgresql.org/download/) and 
* a database client with a graphical user interface such as DBeaver (http://dbeaver.jkiss.org/).

Participants that are not confident or have troubles with the installation can schedule a meeting with the instructor before the seminar to go though the installation.

Instructor

Fotis Psallidas is a PhD Candidate in the Computer Science department supervised by Eugene Wu. His research interests include data visualizations and data provenance for interactive systems. Contact Information:

* Email: fotis@cs.columbia.edu (mailto:fotis@cs.columbia.edu)
* Website: www.cs.columbia.edu/~fotis (http://www.cs.columbia.edu/~fotis)



For this hands-on session, we assume that you have already installed the PostgreSQL database and the DBeaver database client. You need to remember the user and its password that you created during the installation of the PostgreSQL database.

1. **CREATE AND POPULATE THE FLIGHTSDB DATABASE**
2. **CONNECT TO THE FLIGHTSDB DATABASE USING DBEAVER**
3. **EXPLORING THE SCHEMA OF THE DATABASE**
4. **SELECT-FROM-WHERE QUERIES**
5. **AGGREGATE QUERIES**
6. **NESTED QUERIES**

1. CREATE AND POPULATE THE FLIGHTSDB DATABASE

**Step 1. Open a terminal and run the following command to create the flightsdb database**

psql -U postgres -c "CREATE DATABASE flightsdb";

Explanation:

* psql is a command line utility that serves as a database client. It was installed in your system along with PostgresSQL.
* the **-c** option sets the SQL query we want to run against the database. In this case, CREATE DATABASE flightsdb creates a new database with name flightsdb.
* the **-U **option sets the user on whose behalf we run the query. In this case** postgres **is the user that you should have created during the installation of the PostgresSQL server**. **If you set a different user name then use the one you specified instead of postgres**.**

Typically this command will prompt you for a password. You need to type in the password you set during the installation.

**Step 2. Populate the database**

Download the flightsdb.sql file from


psql -U postgres -c "CREATE DATABASE flightsdb";



**2. CONNECT TO THE FLIGHTSDB DATABASE USING DBEAVER**

**Step 1.** Open DBeaver and then go under **File > New. **
[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/j1ZEhNpWKSO_-L5zO_Qsfg]
**Step 2.** In the wizard that opens up select **DBeaver → Database Connection** and press **Next.**
[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/uFlhyiHqbFZKiVxUxqz75g]

**Step 3. **In the “Select new connection type” wizard that opens up select PostgresSQL and press **Next**.
[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/AmpdXFuLF3STH9xHF47nbw]
**Step 4.** Now, the “PostgresSQL Connection Settings” wizard opens up.

* In the Database field enter **flightsdb**

* In the User and Password fields enter the user and  password that you used to create the flights database (e.g., User: postgres and Password: postgres if you followed the default installation of PostgreSQL).

[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/Gk2_tJjzJ9JpMVfTQ-3vdg]

**Step 5.** In the Network wizard that opens up just press **Next**.
[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/FNaVByeeAp4Ra2CUL57klA]**Step 6.** This opens up the “Finish connection creation wizard”. Press **Finish** to create the connection.
[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/A4yLtZRC2nbdVyyYV-oHfA]**Step 7.** If everything went fine so far, then at the left hand side on the connections pane a new entry “Postgres - flightsdb” has been created. Double click on the entry to establish an actual connection to the PostgresSQL server. Incidentally, a new SQL editor to write our queries is also created.

[Image: https://amalgam.quip.com/-/blob/YWQAAAHaltL/bnnl_EEnhqx_J5qWSAeFPQ]
3. EXPLORE THE SCHEMA OF THE DATABASE

Having established the connection with the PostgreSQL server in DBeaver, we are now set to start issuing queries. But we don't even know the schema of the database yet. 

**Solution 1.** On the left hand side on the connection pane click the arrow next to the “PostgresSQL - flightsdb” entry. in the drop down list that appears follow the path **flightsdb**→ **Schemas**→ **public**→ **Tables**. Under **Tables **we can all the table that we have in the database. Now, if we click on the arrow of a table we can also see the columns of the table. Using this traversal exhaustively we can navigate across all tables and find out the schema of the tables. Typically, the names of the tables and columns are indicative of their actual meaning. 

**Solution 2. **Another graphical way to explore the schema of the database is by creating a diagram from the actual database. Follow the path **flightsdb**→ **Schemas **as above and right click on **Schemas. **Then,** **select the **View Schemas **option. At the right hand side two tabs, namely, Properties and Diagram, have opened. Click on the Diagram tab. In this visual, we can see, among other, the tables of the database, their columns, and the Primary Key - Foreign Key relationships effectively getting a better understanding of the schema.
