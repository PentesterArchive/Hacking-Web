# Server Side Template Injection(SSTI)
## What is SSTI?
Server-side template injection is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side.<br />
Server-side template injection attacks can occur when user input is concatenated directly into a template, rather than passed in as data. This allows attackers to inject arbitrary template 
directives in order to manipulate the template engine, often enabling them to take complete control of the server.
 
## SSTI types
We can distinguis two Server Side Template Injections:
  - [Plaintext Context SSTI][Plaintext Context SSTI.].
  - [Code Context SSTI](Code Context SSTI.)
  - 
### Plaintext Context SSTI.
#### Detection
First of all we have to see if we can manage the output:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ea03795b-356a-47f6-83a4-0b103c923d63" width="600"><br /><br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7a747c61-5e2f-4e6a-a8c9-bc50993aa84e" width="600"><br />
After that we have to check if the server evaluate the next expressions.<br />
```python3
{{7*7}}
${{7*7}}
${7*7}
#{7*7}
<%= 7*7 %>
```
***If the expressions are interpreted we can proceei to do a [Plaintext Context SSTI exploitation.](https://github.com/alejandro-pentest/Hacking-Web/blob/main/Server%20Side%20Template%20Injection%20(SSTI)/Server%20Side%20Template%20Injection%20(SSTI).md)***



### Code Context SSTI.




