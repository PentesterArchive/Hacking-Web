# Broken access control
Broken access control is a type of vulnerability that ***allows unauthorized users to gain access to sensitive data or systems*** or ***even perfom actions without permissions***.<br />
We can distinguish some of its parts as follows:
- ***Authentication*** confirms that the user is who they say they are.
- ***Session management*** identifies which subsequent HTTP requests are being made by that same user.
- ***Access control*** determines whether the user is allowed to carry out the action that they are attempting to perform.

## Accessing to unprotected functionalities:
The idea is to gain access to ***unprotected functionalities*** that we are not permited to access and perform a vertical privilege escalation.<br />
We can try to exploit this vulnerability using Brute Force attacks to Discover sensitive directories like `administrator-panel`, with some luck we can access and run administrator functinalities without any credentials.
In some cases, the administrative URL might be disclosed in other locations, such as the `robots.txt` file
