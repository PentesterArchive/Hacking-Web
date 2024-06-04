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


Type 	Description
Stored (Persistent) XSS 	The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)
Reflected (Non-Persistent) XSS 	
DOM-based XSS 	Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)
