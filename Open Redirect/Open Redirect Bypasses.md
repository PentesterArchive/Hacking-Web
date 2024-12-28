# Open Redirect Bypasses.
## Input Validation.
One of the most common types of validation is the **Input Validation** or **Filter Validation**.<br />
If we want the redirect to work, it must contain the word or full URL specified by the server, In this example, we cannot execute our desired redirection beacause it does not contain "https://www.google.com".

<p align="center">
<img src="https://github.com/user-attachments/assets/e0a83ff7-4b1b-4240-a8a7-6150098528ea" width="800">
</p>

### @ Bypass. 
The @ character is interpreted by browsers in a special way, as it is used in the URL structure to separate the 'username:password' from the host.<br />

```http://username:password@domain.com```
<br />

With this information in mind we will write the **URL that the server needs to work + @ + attackerURL**. <br />
Using this method we will use "https://www.google.com" as the username of our malicous URL.
<p align="center">
<img src="https://github.com/user-attachments/assets/3b1d5087-3fbf-43ba-93a5-3d2b7cd03974" width="800">
</p>
