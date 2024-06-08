# SQL Injection (SQLI).
A SQL injection occurs when an *attacker attempts to pass input that changes the final SQL query sent by the web application to the database, enabling the user to perform other **unintended SQL queries** directly against the database*.<br />
There are many ways to accomplish this. To get a SQL injection to work, the attacker must first *inject SQL code and then subvert the web application logic by changing the original query or executing a completely new one*.<br />
First, the attacker has to inject code outside the expected user input limits, so it does not get executed as simple user input. In the most basic case, this is done by injecting a single quote **(')** or a double quote **(")** *to escape the limits of user input and inject data directly into the SQL query.*<br />
Once an attacker can inject, they have to look for a way to execute a different SQL query. This can be done using SQL code to make up a working query that executes both the intended and the new SQL queries.<br/> 
Finally, *to retrieve our new query's output, we have to interpret it or capture it on the web application's front end.*<br />


## SQL Code.
It's essential to know SQL code to perform effective SQL injection exploitation. If you want to learn more about SQL or need a reference, you can find the following files in my Fundamentals GitHub repository, where I cover the following content:
- [mysql command line tool](https://github.com/alejandro-pentest/Fundamentals/tree/main/SQL%20Basics)
  - [SQL Basic Syntax](https://github.com/alejandro-pentest/Fundamentals/blob/main/SQL%20Basics/SQL%20Basic%20Syntax/Readme.md)
    - [SQL Query](https://github.com/alejandro-pentest/Fundamentals/blob/main/SQL%20Basics/SQL%20Basic%20Syntax/SQL%20Querys.md)

## SQL Injection Types:
<br />
<br />

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b43c55cb-839c-4755-8002-ec348bea0e62" width="800" alt="Sublime's custom image"/>
</p>
<br />

### In-band SQLi.
The attacker uses the same channel of communication to launch their attacks and to gather their results. In-band SQLi’s simplicity and efficiency make it one of the most common types of SQLi attack. There are two sub-variations of this method:<br />

- __Union-based SQLi:__ Is an in-band SQL injection technique that leverages the **UNION SQL** operator to combine the results of two or more SELECT statements into a single result which is then returned as part of the HTTP response.<br>

- __Errorbased SQLi:__ The attacker performs actions that cause the database to produce error messages. The attacker can potentially use the data provided by these error messages to gather information about the structure of the database.<br />




### Inferential SQLi (Blind SQLi).
Inferential SQL Injection, unlike in-band SQLi, may take longer for an attacker to exploit, however, it is just as dangerous as any other form of SQL Injection. In an inferential SQLi attack, no data is actually transferred via the web application and *the attacker would not be able to see the result of an attack in-band* (which is why such attacks are commonly referred to as “blind SQL Injection attacks”). Instead, *an attacker is able to reconstruct the database structure by sending payloads, observing the web application’s response and the resulting behavior of the database server*.<br />
Blind SQL injections can be classified as follows:
- __Blind-boolean-based SQLI:__ The attacker sends a SQL query to the database prompting the application to return a result. The result will vary depending on whether the query is true or false. Based on the result, the information within the HTTP response will modify or stay unchanged. The attacker can then work out if the message generated a true or false result.

- __Blind-time-based SQLI:__ We use SQL conditional statements that delay the page response if the conditional statement returns true using the Sleep() function. The attacker can see from the time the database takes to respond, whether a query is true or false.

### Out-of-band SQLI.
Finally, in some cases, we may not have direct access to the output whatsoever, so we may have to direct the output to a remote location, 'i.e., DNS record,' and then attempt to retrieve it from there. This is known as Out-of-band SQL injection.
Out-of-band SQL Injection is not very common, mostly because it depends on features being enabled on the database server being used by the web application.





