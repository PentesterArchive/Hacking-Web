# Database Enumeration. INFORMATION_SCHEMA database.
To pull data from tables using UNION SELECT and INFORMATION_SCHEMA database, we need to properly form our SELECT queries. To do so, we need the following information:
- [List of databases](#listing-databases)
- [List of tables within each database](#listing-tables)
- [List of columns within each table](#listing-columns)

## Listing Databases.
## SCHEMATA
To start our enumeration, we should find what databases are available on the DBMS. 
1. The table `SCHEMATA` in the `INFORMATION_SCHEMA` database __contains information about all databases on the server__. It is used to obtain database names so we can then query them.
2. The `SCHEMA_NAME` column __contains all the database names currently present__.
Let us first test this on a local database to see how the query is used:
<p align="left">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/62d3148a-8343-4af7-816e-0cca8204cd29" width="600" alt="Sublime's custom image"/>
</p>

> Note: "mysql", "sys", "information_schema", "performance_schema" databases are default MySQL databases and are present on any server, so we usually ignore them during DB enumeration.

Now, let's do the same using a UNION SQL injection, with the following payload:
```sql 
' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
> The last command throws all the results before we can see the databases names, so we can just search for 1 "port" to obtain a smoller a output.
```sql
SG SIN' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/d1ba9883-0ead-4745-8244-62d89a61f9f4" width="550" alt="Sublime's custom image"/>
</p>






We see two databases, "ilfreight" and "dev", apart from the default ones.<br />

### Obtaining the database running in the application.
Let us find out which database the web application is running to retrieve ports data from. We can find the current
database with the `SELECT database()` query. We can do this similarly to how we found the DBMS version in the previous section:
```sql
SG SIN' UNION SELECT 1, database(), 3, 4 -- -
```


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/a10d0f5d-84f8-4071-9184-91dea7db017f" width="600" alt="Sublime's custom image"/>
</p>



We see that the database name is "ilfreight". However, the other database (dev) looks interesting. So, let us try to retrieve the tables from it.


## Listing Tables.

Before we dump data from the "dev" database, we need to get a list of the tables to query them with a `SELECT` statement. To find all tables within a database, we can use the `TABLES` table in the 
`INFORMATION_SCHEMA` Database.
> The `TABLES` table _contains information about all tables throughout the database_. This table contains multiple columns, but we are interested in the `TABLE_SCHEMA` and `TABLE_NAME` columns.<br />
> The `TABLE_NAME` column __stores table names__, while the `TABLE_SCHEMA` column __points to the database each table belongs to__. This can be done similarly to how we found the database names.
For example, we can use the following payload to find the tables within the "dev" database:
```sql
sg' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```
> Note: how we replaced the numbers '2' and '3' with 'TABLE_NAME' and 'TABLE_SCHEMA', to get the output of both columns in the same query.
> Note: we added a (where table_schema='dev') condition to only return tables from the 'dev' database, otherwise we would get all tables in all databases, which can be many.

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/71b644dc-15cc-4f0f-b7e7-bc0e7c992849" width="600" alt="Sublime's custom image"/>
</p>

We see four tables in the dev database, namely credentials, framework, pages, and posts. For example, the credentials table could contain sensitive information to look into it.


### Listing Columns.
To dump the data of the credentials table, we __first need to find the column names in the table__, which can be found in the `COLUMNS` table in the `INFORMATION_SCHEMA` database. 
The `COLUMNS` table _contains information about all columns present in all the databases_. This helps us find the column names to query a table for. The `COLUMN_NAME`, `TABLE_NAME`, and `TABLE_SCHEMA`
columns can be used to achieve this. As we did before, let us try this payload to find the column names in the credentials table:

```sql
sg' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/101f99a8-fc84-4e11-ad38-cca778ebf851" width="600" alt="Sublime's custom image"/>
</p>


> The table has two columns named username and password. We can use this information and dump data from the table.


### Dumping Data.
Now that we have all the information, __we can form our UNION query to dump data of the username and password columns from the credentials table in the dev database__. 
We can place username and password in place of columns 2 and 3:
```sql
sg' UNION select 1, username, password, 4 from dev.credentials-- -
```
> Remember: don't forget to use the dot operator to refer to the 'credentials' in the 'dev' database, as we are running in the 'ilfreight' database, as previously discussed.
We were able to get all the entries in the credentials table, which contains sensitive information such as password hashes and an API key.

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/fb21b92a-79f8-4c57-a58c-4ef9e4a6a5c6" width="600" alt="Sublime's custom image"/>
</p>






