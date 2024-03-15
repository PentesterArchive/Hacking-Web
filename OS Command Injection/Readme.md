# OS Command Injection.
OS Command Injection is a vulnerability that allows us to execute System Commands on a target via Web Application. 

## How does it work?
In some cases the target server requires to manage some functionalities executing scripts and there we can maybe have a chance to execute our commands.<br />
The idea is that the target can execute for example `checkstock.sh` to check the stock, the web has to send some parameters like the `product id`. We can try to exploit those parameters.
If we go to or own system we can try to execute a script and see what happen if we execute it using some operators with another command behind:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/1772183b-e495-4312-8b64-38a49f4e19b7" width="300"><br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/1aa04a50-857a-44e2-887c-54d1f98489f1"><br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/94da0f7b-d3fb-4310-933b-797d0c1bc0c2" width="300"><br />
***We can use those methods to obtain all the system info that we need or even to get a reverse shell:***
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/fb13d86d-c6f4-4d1f-94b6-f7766d32088c" width="600">

## Executing a basic OS Command Injection.


