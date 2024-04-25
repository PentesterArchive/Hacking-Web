# XML External Entity Injection (XXE).
## ¿What is an XML Injection?
XML External Entity Injecton (XXE) it's a web vulnerability that allows an attacker to interfere in the XML data. <br />
Using this vulnerability we can see Files from the target system or interact with our own system via web server. <br />
Sometimes we can escalate an XXE to an [SSRF - Server Side Request Forgery](https://github.com/alejandro-pentest/Hacking-Web/tree/main/Server-side%20request%20forgery%20(SSRF)).

## XXE Exploitation.
If we fill and send the data, we can see that the parameter email is being outputed back by the server.<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c60143c4-bda3-4a60-82a6-860ab2410da4" width="350">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/5232d815-f5c9-4423-b751-2d7f422ee09a" width="1000">

![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/5232d815-f5c9-4423-b751-2d7f422ee09a)

We have to check if we can also send the outup using the next method:<br />
```xml
<!DOCTYPE foo [<!ENTITY <Name> "<Value>"> ]>
```
![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/f557d264-8ae4-4d03-b479-8f2fcaf13f52)<br />

If the last command was useful we can now try to report some critical files using some Wrappers.<br />
![4](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/eac5fa56-da99-429f-9d50-4168c56bac44)

### Wrappers.
