# Server Side Template Injection (SSTI)
## Detection.
The first task we have to do to detect a SSTI vulnerability is to check which is the template that is being used by the server. After that we have two possible ways:
- We extract the server template so we know exactly the programming lenguaje that is being used by the server.
- We have to try the next expressions to realize which one of them are interpreted so we can exactly know which programming lenguage we have to use to deploy our payload.
```javascript
{{7*7}} 
${{7*7}} 
${7*7} 
#{7*7} 
<%= 7*7 %>
```
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/86a348ed-e521-412d-8e41-3a0b3877dce6" width="600"><br />

After that we can check some resources as ***1- [PayloadsAllTheThings-SSTI](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)*** or ***2 - [Hacktricks-SSTI](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)***.
