---
layout: template1
title: createdb
comments: false
---

<div class="jumbotron" style="background-color: #F4FDFD">
    <p style="text-align: justify">In this tutorial, you will first learn how to create and populate a database. Then, you will learn how tow connect to a database from a database client with a graphical user interface. Finally, you will learn how to explore the schema of the database visually.</p>

    <p>
    <h2>Contents</h2>
    <ol>
    	<li> <a href="#create">CREATE AND POPULATE THE FLIGHTSDB DATABASE</a></li>
		<li> <a href="#dbeaver">CONNECT TO THE FLIGHTSDB DATABASE USING DBEAVER</a></li>
		<li> <a href="#explore">EXPLORE THE SCHEMA OF THE DATABASE VISUALLY</a></li>
	</ol>
	</p>
</div>







### <a name="create"></a>1. CREATE AND POPULATE THE FLIGHTSDB DATABASE

**Step 1.** Open a terminal and run the following command to create the flightsdb database**

```
psql -U postgres -c "CREATE DATABASE flightsdb";
```

Explanation:

* psql is a command line utility that serves as a database client. It was installed in your system along with PostgresSQL.

* the **-c** option sets the SQL query we want to run against the database. In this case, **CREATE DATABASE flightsdb** creates a new database with name **flightsdb**.

* the **-U** option sets the user on whose behalf we run the query. In this case, **postgres** is the user that you should have created during the installation of the PostgresSQL server. If you set a different user name then use the one you specified instead of postgres.

Typically this command will prompt you for a password. You need to type in the password you set during the installation.

**Step 2.** Populate the database

* Download the ```flightsdb.sql``` file from 

* Now, using a terminal, navigate to the folder where flightsdb.sql was downloaded and use the following command to populate the database.

```
psql -U postgres -c < flightsdb.sql;
```

### <a name="dbeaver"></a>2. CONNECT TO THE FLIGHTSDB DATABASE USING DBEAVER

**Step 1.** Open DBeaver and then go under **File → New**.

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/j1ZEhNpWKSO_-L5zO_Qsfg)

**Step 2.** In the ``Select a wizard`` window that opens up select **DBeaver → Database Connection** and press **Next.**

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/uFlhyiHqbFZKiVxUxqz75g)

**Step 3.** In the ``Select new connection type`` wizard that opens up select PostgresSQL and press **Next**.

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/AmpdXFuLF3STH9xHF47nbw)

**Step 4.** Now, the ``PostgresSQL Connection Settings`` wizard opens up.

* In the Database field enter **flightsdb**

* In the User and Password fields enter the user and  password that you used to create the flights database (e.g., User: postgres and Password: postgres if you followed the default installation of PostgreSQL).

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/Gk2_tJjzJ9JpMVfTQ-3vdg)

**Step 5.** In the ``Network wizard`` that opens up just press **Next**.

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/FNaVByeeAp4Ra2CUL57klA)

**Step 6.** This opens up the ``Finish connection creation wizard``. Press **Finish** to create the connection.

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/A4yLtZRC2nbdVyyYV-oHfA)

**Step 7.** If everything went fine so far, then at the left hand side on the connections pane a new entry ``Postgres - flightsdb`` has been created. Double click on the entry to establish an actual connection to the PostgresSQL server. Incidentally, a new SQL editor to write our queries is also created.

![alt text](https://amalgam.quip.com/-/blob/YWQAAAHaltL/bnnl_EEnhqx_J5qWSAeFPQ)

### <a name="explore"></a>3. EXPLORE THE SCHEMA OF THE DATABASE

Having established the connection with the PostgreSQL server in DBeaver, we are now set to start issuing queries. But we don't even know the schema of the database yet. 

**Solution 1.** Explore the schema using the connection pane on the left hand side.

* On the left hand side on the connection pane click the arrow next to the ``PostgresSQL - flightsdb`` entry. 

* In the drop down list that appears follow the path **flightsdb**→ **Schemas**→ **public**→ **Tables**. Under **Tables**, we can see all the tables that exist in the database. 

* Now, if we click on the arrow of a table we can also see the columns of the table. Using this traversal exhaustively we can effectively explore the schema of the database. Typically, the names of tables and columns are indicative of their actual meaning. 

**Solution 2.** Create a diagram of the database.

* Another graphical way to explore the schema of the database is by creating a diagram from the actual database.

* Follow the path **flightsdb** → **Schemas** as above.

* Right click on **Schemas**. Then, select the **View Schemas** option.

* At the right hand side two tabs, namely, Properties and Diagram, have opened up. 

* Click on the ``Diagram`` tab. In this visual, we can see, among others, the tables of the database, their columns, and the Primary Key - Foreign Key relationships effectively getting a better understanding of the schema.
