# Open Redirect
## What is Open Redirect?
An open redirect vulnerability occurs when an application allows a user to control a redirect or forward to another URL. If the app does not validate untrusted user input,
an attacker could supply a URL that redirects an unsuspecting victim from a legitimate domain to an attacker’s phishing site.

### Basic Open Redirect Exploitation.
In the following image, we can see that hovering our mouse over the button, we are redirected to *https://google.com*. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/54b1a8a6-6534-4782-aa1a-6454771c53de" width="700">
</p>

We can copy the full URL "Copy link" and modify the redirection to point to our malicious server.
We can perform the same action using Burpsuite. Regardless of the method, if the redirection is not sanitized, we will be redirected to the specified domain.

<p align="center">
<img src="https://github.com/user-attachments/assets/3e01a247-b1d0-4688-b89c-87dcc6cf6162" width="700">
<img src="https://github.com/user-attachments/assets/d285fa92-aae5-48e6-a707-728f292bc31b" width="700">
</p>

### Open Redirect Sanitization.
There are several ways to sanitize an Open Redirect and the following are some of the possible bypasses we can use: 
[Open Redirect Bypasses](https://github.com/PentesterArchive/Hacking-Web/blob/main/Open%20Redirect/Open%20Redirect%20Bypasses.md).
