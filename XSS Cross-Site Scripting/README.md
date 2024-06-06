# XSS Cross-Site Scripting 
## What is XSS?
When a vulnerable web application does not properly sanitize user input, a malicious user can inject extra JavaScript code in an input field (e.g., comment/reply), so once another user views the same 
page, they unknowingly execute the malicious JavaScript code.</br>
XSS vulnerabilities are solely executed on the client-side and hence do not directly affect the back-end server. They can only affect the user executing the vulnerability. 
The direct impact of XSS vulnerabilities on the back-end server may be relatively low, but they are very commonly found in web applications.

## XSS Types
We can distinguis XSS attacks by their persistence: __Persistent XSS__ (Stored XSS) __Non Persistent XSS__ (Reflected XSS, DOM Based XSS):

| Tipo                                   | Descripción                                                                                                                                                                                                                                                                                                      |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Stored XSS __(Persistent)__       | The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)          |
| Reflected XSS __(Non-Persistent)__     | Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)                                                                                            |
| DOM-based XSS __(Non-Persistent)__ | Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags) |

## Detecting XSS vulnerabilities.
### Automatic detection.
We can use several automated tools to detect XSS vulnerabilities, pe: [xsser](https://github.com/epsylon/xsser) or [XSS Strike](https://github.com/s0md3v/XSStrike).

### Manual detection.
When it comes to manual XSS discovery, the difficulty of finding the XSS vulnerability depends on the level of security of the web application. Basic XSS vulnerabilities can usually be found through testing various XSS payloads, but identifying advanced XSS vulnerabilities requires advanced code review skills. </br>
#### XSS Basic vulnerability
```javascript
<script>alert(window.origin)</script>
```
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/a82237cb-c6d8-4e2b-bc1d-ab4256d4a205" width="450">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/aa0a4bc8-54bc-485e-a4f5-08fafd5d21b5" width="450">

### XSS Bypass
If we try the previous XSS injection it may fail even if the target is vulnerable, the reason is that maybe the injection is a bit sanitized, in that case we can try
to input other tags like:
```javascript
<img src="" onerror=alert(window.origin)>
```
```javascript
<plaintext>alert(window.origin)</plaintext>
```
```javascript
\<a onmouseover="alert(document.cookie)"\>xxs link\</a\> 
```
#### XSS filter evasion.
XSS filters are one of the primary methods used to prevent all types of XSS vulnerabilities. They work by finding patterns that are typical of XSS attack vectors and removing such suspicious code from user input data. Patterns are typically defined using regular expressions. 
The following are the most common methods used by attackers in their malicious code to fool XSS filters. Of course, all these methods may be combined or refined. 
1. Encode characters.
2. Case manipulation:
> ```html<script>,<SCRIPT>,<sCrIpT>```
3. Modern web browsers usually ignore extra whitespace characters:
> ```html<script   >, <script[\x09]>, <script[\x13]>....``` We can also attempt to insert a null byte anywhere in the string, for example, ```html<scr[\x00]ipt>```.
4. Using atipical web handlers, we can find a lot of them on [W3School.](https://www.w3schools.com/jsref/dom_obj_event.asp).


<br />


**We can find more filter evasioons in** [Invicti XSS Filter Evasion.](https://www.invicti.com/learn/xss-filter-evasion/)

**XSS BYPASSES CHEAT SHEET ->** [owasp - XSS_Filter_Evasion_Cheat_Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)

## XSS Attacks.
XSS can lead to different types of attack depending on the XSS vulnerability type we against:<br/> 
**Stored XSS** -> [XSS Defacing Attack.](https://github.com/alejandro-pentest/Hacking-Web/blob/main/XSS%20Cross-Site%20Scripting/XSS%20ATTACKS/XSS%20Defacing%20Attacks.md)
[XSS Phishing Attack.](https://github.com/alejandro-pentest/Hacking-Web/blob/main/XSS%20Cross-Site%20Scripting/XSS%20ATTACKS/XSS%20Phishing%20attack.md)





