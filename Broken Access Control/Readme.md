# Broken access control
Broken access control is a type of vulnerability that ***allows unauthorized users to gain access to sensitive data or systems*** or ***even perfom actions without permissions***.<br />
We can distinguish some of its parts as follows:
- ***Authentication*** confirms that the user is who they say they are.
- ***Session management*** identifies which subsequent HTTP requests are being made by that same user.
- ***Access control*** determines whether the user is allowed to carry out the action that they are attempting to perform.

-------------

## Accessing to unprotected functionalities:
The idea is to gain access to ***unprotected functionalities*** that we are not permitted to access and perform a vertical privilege escalation.<br />
We can try to exploit this vulnerability using Brute Force attacks to discover sensitive directories like `administrator-panel`, with some luck, we can access and run administrator functionalities without any credentials.<br />
In some cases, the administrative URL might be disclosed in other locations, such as the `robots.txt` file.

------------------------

## Unprotected admin functionality with unpredictable URL
In some cases, sensitive functionality are located at an unpredictable location by giving it a less predictable URL. This is an example of so-called "security by obscurity". However, hiding sensitive functionality does not provide effective access control because users might discover the obfuscated URL in a number of ways. For example we can find links to the admin-panel in the JavaScript of the webpage.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/961daed4-1d15-4a25-b067-0b1ed105f07e" width=60% height=60%>

-----------------

## Parameter-based access control methods
Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:
- Hidden field.
- Cookie.
- Preset query string parameter. <br />
<br />
The application makes access control decisions based on the submitted value. For example:<br />

```
https://insecure-website.com/login/home.jsp?admin=true 
https://insecure-website.com/login/home.jsp?role=1
```

This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as administrative functions.

### Cookie 

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/9672f343-784d-4e0d-84c3-a6388227a665" width="900" height="150">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/58438b0a-23a3-4b7f-a5e4-225e557e197c" width=900 height=150>

----------------------------------------------------

## Horizontal privilege escalation
Horizontal privilege escalation occurs if a user is able to gain access to resources belonging to another user, instead of their own resources of that type. For example, if an employee can access the records of other employees as well as their own, then this is horizontal privilege escalation. <br />
Horizontal privilege escalation attacks may use similar types of exploit methods to vertical privilege escalation. For example, a user might access their own account page using the following URL:<br />
`https://insecure-website.com/myaccount?id=123`<br />

If an attacker modifies the id parameter value to that of another user, they might gain access to another user's account page, and the associated data and functions.<br />

**In some applications, the exploitable parameter does not have a predictable value**. For example, instead of an incrementing number, an application might use globally unique identifiers (GUIDs) to identify users. This may prevent an attacker from guessing or predicting another user's identifier. However, ***the GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews***.








