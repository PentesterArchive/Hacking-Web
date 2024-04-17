# Log Poisoning.
In any server there are some files that register the logs. When we find a [Local File Inclusion](https://github.com/alejandro-pentest/Hacking-Web/tree/main/Local%20File%20Inclusion-Path%20traversal) vulnerability we can try to write some code in them and then try to execute it.

## Detection.
To detect if we are able to do a Log Poisoning we have to try to deploy any log file.
> Log server files are usually stored in `/var/log`:<br />
- `/var/log` in the target system:
![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e41f5f0d-94f3-40d4-8614-77c4cb7fd76c)
- Deploying `/var/log/apache2/acces.log` taking advantage of a LFI vulnerability:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/76c1609e-2a9c-4596-9efb-89b2f269749c" width="30000"><br />
- Any action that we commit in the server will be registered there. <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/2b395c06-939d-43b7-9861-4dbc95ff5795" width="2500">

## Exploitation.
We can try to modify the `User-Agent` of our requests to visualize in the logs the information we want. <br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c002b371-ca10-4a5a-927e-ff92cbb5e311" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/684b6e72-457b-4f10-a8c9-2a652dafd37c" width="2000">

### Executing commands: 
- If we try to inyect some code in the log the server should execute it as we can see in the following example: <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/832b87ff-b54d-408a-93cf-7de6c2fa5f73" width="600">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8a83e19d-a9db-4db9-a902-09b4f662e09c" width="2500">

### Reverse shell: 
- Using the previous method we can obtain a reverse shell.
> It's important to execute insert the poison as follows because we have to scape the character `$`. If we don't deploy a good input we can break the Log Poisoning technique and we may not be able to exploit it. `"User-Agent: <?php system(\$_GET['cmd']); ?>"`.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/df8723f6-9ae5-446a-915c-e123f95a199a" width="600">

![10](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b9587167-cec8-4811-91ed-0ec7570a4e42)







