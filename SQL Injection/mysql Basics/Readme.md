# mysql command line tool.
The mysql utility is used to authenticate to and interact with a MySQL/MariaDB database. The `-u` flag is used to supply the username and the `-p` flag for the password. 



==The -p flag should be passed empty, so we are prompted to enter the password and do not pass it directly on the command line since it could be stored in cleartext in the bash_history file.==

Other DBMS users would have certain privileges to which statements they can execute. We can view which privileges we have using the `SHOW GRANTS`
command.

When we do not specify a host, it will default to the localhost server. We can specify a remote host and port using the -h and -P flags.
Intro to MySQL

`mysql -u root -h docker.hackthebox.eu -P 3306 -p`




Note: The default MySQL/MariaDB port is (3306), but it can be configured to another port. It is specified using an uppercase `P`, unlike the lowercase `p` used for passwords.


<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7728c41b-5fc2-4b82-bee2-66c58570ec23" width="700">


![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7728c41b-5fc2-4b82-bee2-66c58570ec23)

![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/774fa9c3-649a-45dd-a1cd-7dd4f6787a76)

![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/689df40f-11dd-4bd6-91a7-eca2ae8a3124)

![4](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/04e59813-fd27-41c5-aadb-df454a5ff8f8)

![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/813cbcd5-5f1b-4af5-902c-6105ec7d8ba3)


![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/17fd836d-a020-439e-b3c5-df11d838beba)
