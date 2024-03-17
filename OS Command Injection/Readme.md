# OS Command Injection.
OS Command Injection is a vulnerability that allows us to execute System Commands on a target via Web Application. 

## How does it work?
In some cases the target server requires to manage some functionalities executing scripts and there we can maybe have a chance to execute our commands.<br />
The idea is that the target can execute for example `checkstock.sh` to check the stock, the web has to send some parameters like the `product id`. We can try to exploit those parameters.
If we go to or own system we can try to execute a script and see what happen if we execute it using some operators with another command behind:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/1772183b-e495-4312-8b64-38a49f4e19b7" width="300">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/1aa04a50-857a-44e2-887c-54d1f98489f1">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/94da0f7b-d3fb-4310-933b-797d0c1bc0c2" width="300"><br />
***We can use those methods to obtain all the system info that we need or even to get a reverse shell:***
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/fb13d86d-c6f4-4d1f-94b6-f7766d32088c" width="500">

## Executing a basic OS Command Injection.
In a basic OS Command Injection we need to find an action being performed by the web and try to run our commands using the operators used in the previous examples.<br />
In the next case the target is executing `checkstock.sh` to check the stock. We can try to make the command injection there:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/5d66821f-de95-4ab8-8ce5-4ef80b07956d" width="800"><br />

## Bind OS Command Injection.
In a Bind OS Command Injection the server executes our commands but we can not see the output as we did in the previous case. However, ***we can execute commands that we know take some time to finish***, as for example we can execute a `ping -c X localhost`, of course ***if the web page take some time its because the command is being executed*** and after thah we can try to obtain a reverse shell whith netat or we can even redirect our command output to a .txt and try to visualize it on the web.<br />

### Time Command Injection:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/de258830-5ef9-484d-9081-27f3efb4dd27" width="700"><br />
I used the operator `||` 'OR' because the script that the web was executing is not running properly due to our command injection. The vulnerable parameter is "email," and the rest of the parameters cannot be sent as intended.

### Redirecting a Bind OS Command Injection:
If we are against a Bind OS Command Injection we can try to create a .txt redirecting the command output. If we do this its essential that we can find a way to show those files. In the next example we have the directory /var/www/images, which contains the web images and its writable, so if we open an image from the web site we can see this:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b08da3e6-7853-4fcc-aead-c6b2c71f5590" width="700"><br />
Now we can try to make the file with our output and finally see it:<br />

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/777518e1-3d2e-4cbe-881c-7b19295e8d16" width="600">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/4e332df8-c71e-44e9-ab4d-9c3d06658fcc" width="600"><br />
Once we have obtained the username we can attempt, for example, to retrieve the `id_rsa`. with the command `cat /home/<username>/.ssh/id_rsa > /var/www/images/id_rsa.txt`. If the command was successful we can now access the target system via ssh.





