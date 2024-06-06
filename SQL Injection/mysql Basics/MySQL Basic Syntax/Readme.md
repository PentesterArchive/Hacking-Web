# MySQL Basic Syntax.
## Creating DataBase.
`CREATE DATABASE <DBName>;`
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

### SELECT
### DROP
### ALTER
