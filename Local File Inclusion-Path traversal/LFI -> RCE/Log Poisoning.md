# Log Poisoning.
In any server there are some files that register the logs. When we find a [Local File Inclusion](https://github.com/alejandro-pentest/Hacking-Web/tree/main/Local%20File%20Inclusion-Path%20traversal) vulnerability we can try to write some code in them and then try to execute it.

## Detection.
To detect if we are able to do a Log Poisoning we have to try to deploy any log file.
> Log server files are usually stored in `/var/log`:<br />
- `/var/log` in the target system:
![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e41f5f0d-94f3-40d4-8614-77c4cb7fd76c)
- Deploying `/var/log/apache2/acces.log` taking advantage of a LFI vulnerability:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/76c1609e-2a9c-4596-9efb-89b2f269749c" width="30000">
- Any action that we do in the server will be registered there.
![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ac446a20-f1f6-47cd-aa01-fa1712263226)

## Exploitation.
We can try to modify the `User-Agent` of our requests to visualize in the logs the information we want. <br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c002b371-ca10-4a5a-927e-ff92cbb5e311" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/684b6e72-457b-4f10-a8c9-2a652dafd37c" width="2000">

### Executing commands: 
- If we try to inyect some code in the log the server should execute it as we can see in the following example:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/832b87ff-b54d-408a-93cf-7de6c2fa5f73" width="600">
![7](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/026d1fb9-baa8-4acd-98e0-9b67c4abd602)

### Reverse shell: 
- Using the previous method we can obtain a reverse shell.
> It's important to execute insert the poison as follows because we have to scape the character `$`. If we don't deploy a good input we can break the Log Poisoning technique and we may not be able to exploit it. `"User-Agent: <?php system(\$_GET['cmd']); ?>"`.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/df8723f6-9ae5-446a-915c-e123f95a199a" width="600">

![10](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b9587167-cec8-4811-91ed-0ec7570a4e42)







