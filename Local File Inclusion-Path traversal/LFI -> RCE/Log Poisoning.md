# Log Poisoning.
In any server, there are files that register logs. When we find a [Local File Inclusion](https://github.com/alejandro-pentest/Hacking-Web/tree/main/Local%20File%20Inclusion-Path%20traversal) vulnerability we can attempt to inject code into these files and then execute it.

## Detection.
To detect if we can perform Log Poisoning, we need to attempt to manipulate any log file.
> Log server files are tipically stored in `/var/log`:<br />
- `/var/log` on the target system:
![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e41f5f0d-94f3-40d4-8614-77c4cb7fd76c)
- Deploying `/var/log/apache2/acces.log` taking advantage of a LFI vulnerability:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/76c1609e-2a9c-4596-9efb-89b2f269749c" width="30000"><br />
- Any action we perform on the server will be logged there. <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/2b395c06-939d-43b7-9861-4dbc95ff5795" width="2500">

## Exploitation.
We can attempt to modify the `User-Agent` of our requests to visualize in the logs the information we want. <br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c002b371-ca10-4a5a-927e-ff92cbb5e311" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/684b6e72-457b-4f10-a8c9-2a652dafd37c" width="2000">

### Executing commands: 
- If we try to inject some code in the log, the server may execute it, as we can see in the following example: <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/832b87ff-b54d-408a-93cf-7de6c2fa5f73" width="600"> <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8a83e19d-a9db-4db9-a902-09b4f662e09c" width="2500">

### Reverse shell: 
- Using the previous method, we can obtain a reverse shell.
> It's important to insert the poison as follows because we need to scape the character `$`. If we provide an incorrect input, we can break the Log Poisoning technique and we may not be able to exploit it. `"User-Agent: <?php system(\$_GET['cmd']); ?>"`. <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/df8723f6-9ae5-446a-915c-e123f95a199a" width="600"> ![10](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b9587167-cec8-4811-91ed-0ec7570a4e42)







