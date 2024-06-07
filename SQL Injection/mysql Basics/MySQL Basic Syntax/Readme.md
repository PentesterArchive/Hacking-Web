# MySQL Basic Syntax.
## Creating DataBase.
`CREATE DATABASE <DBName>;`

```sql
mysql> CREATE DATABASE shop;
Query OK, 0 rows affected (0.03 sec)
```

## Creating Tables.
DBMS stores data in the form of tables. A table is made up of horizontal rows and vertical columns. The intersection of a row and a column is called a cell. Every table is created with a fixed set of columns, where each column is of a particular data type.
```sql
mysql> CREATE TABLE logins (
    ->     id INT,
    ->     username VARCHAR(100),
    ->     password VARCHAR(100),
    ->     date_of_joining DATETIME
    ->     );
Query OK, 0 rows affected (0.03 sec)
```
### Table Properties.
```sql
mysql > CREATE TABLE logins (
  ->    id INT NOT NULL AUTO_INCREMENT,
  ->    username VARCHAR(100) UNIQUE NOT NULL,
  ->    password VARCHAR(100) NOT NULL,
  ->    date_of_joining DATETIME DEFAULT NOW(),
  ->    PRIMARY KEY (id)
  ->    );
```

## SQL Statements.
### INSERT
The INSERT statement is used to add new records to a given table. The statement following the below syntax:<br />
```sql
mysql> INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02');
Query OK, 1 row affected (0.00 sec)
```

The example above shows how to add a new login to the logins table, with appropriate values for each column. However, we can skip filling columns with default values, such as id and date_of_joining. This can be done by specifying the column names to insert values into a table selectively:
Code: sql
```sql
INSERT INTO table_name(column2, column3, ...) VALUES (column2_value, column3_value, ...);
```

> Note: skipping columns with the 'NOT NULL' constraint will result in an error, as it is a required value.

```sql
mysql> INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss');
Query OK, 1 row affected (0.00 sec)
```

We can also insert multiple records at once by separating them with a comma:
```sql
mysql> INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');

Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0
```
The query above inserted two new records at once.





### SELECT
### DROP
### ALTER
