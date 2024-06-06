# mysql command line tool.
The mysql utility is used to authenticate to and interact with a MySQL/MariaDB database. The `-u` flag is used to supply the username and the `-p` flag for the password. 
> The -p flag should be passed empty, so we are prompted to enter the password and do not pass it directly on the command line since it could be stored in cleartext in the bash_history file.
The default MySQL/MariaDB port is (3306), but it can be configured to another port. It is specified using an uppercase `P`, unlike the lowercase `p` used for passwords.<br />
`-u` **->** Username.<br />
`-p` **->** Password.<br />
`-h` **->** Host. <br />
`-P` **->** Port.<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/9286e439-9d67-4de1-bae9-196331f6b5d2" width="700">

When we do not specify a host, it will default connect to the localhost server.

We can view which privileges we have using the `SHOW GRANTS` command. (in this case we have full privs cause we are logged as root).

![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/774fa9c3-649a-45dd-a1cd-7dd4f6787a76)

<br />`SHOW DATABASES` **->** Will display all the databases that this db is containing.<br />
![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/689df40f-11dd-4bd6-91a7-eca2ae8a3124)
<br /> After seen which databases does this database contains, we can select or "use" one of them `use <DBName>;`.<br />
![7](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/254c36e4-d829-4e3e-90e2-80fceb29c247)
<br /> Once this database is selected we can list all the tables from the database `SHOW TABLES;`<br />
![4](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/04e59813-fd27-41c5-aadb-df454a5ff8f8)
<br />Now is the time to finally see what secrets this database contains. We can use `describe <table>;` to retrieve all the information we can extract.<br />
![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/813cbcd5-5f1b-4af5-902c-6105ec7d8ba3)
<br /> If we want to extract all the data from the table we can use `select * FROM TABLE;`.
![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/17fd836d-a020-439e-b3c5-df11d838beba)
