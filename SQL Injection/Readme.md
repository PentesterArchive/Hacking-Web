# SQL Injection (SQLI).
A SQL injection occurs when an attacker attempts to pass input that changes the final SQL query sent by the web application to the database, enabling the user to perform other unintended SQL queries directly 
against the database.
There are many ways to accomplish this. To get a SQL injection to work, the attacker must first inject SQL code and then subvert the web application logic by changing the original query or executing a completely new one.
First, the attacker has to inject code outside the expected user input limits, so it does not get executed as simple user input. In the most basic case, this is done by injecting a single quote (') or a double quote (")
to escape the limits of user input and inject data directly into the SQL query.
Once an attacker can inject, they have to look for a way to execute a different SQL query. This can be done using SQL code to make up a working query that executes both the intended and the new SQL queries. There are many ways to achieve this, like using stacked queries or using Union queries. Finally, to retrieve our new query's output, we have to interpret it or capture it on the web application's front end.
There are so many things that we can do when we find an SQL Injection, such as bypass an autentication or even retrieve database information.

## SQL Code.
It's essential to know SQL code to perform effective SQL injection exploitation. If you want to learn more about SQL or need a reference, you can find the following files in my Fundamentals GitHub repository, where I cover the following content:
- [mysql command line tool](https://github.com/alejandro-pentest/Fundamentals/tree/main/SQL%20Basics)
  - [SQL Basic Syntax](https://github.com/alejandro-pentest/Fundamentals/blob/main/SQL%20Basics/SQL%20Basic%20Syntax/Readme.md)
    - [SQL Query](https://github.com/alejandro-pentest/Fundamentals/blob/main/SQL%20Basics/SQL%20Basic%20Syntax/SQL%20Querys.md)
