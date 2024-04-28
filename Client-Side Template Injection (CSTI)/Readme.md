# Client-Side Template Injection (CSTI).
## What is CSTI?
Client-side template injection is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed client-side.

## Detection.
We can detect a CSTI vulnerability using the same expressions as we use in a [Server Side Template Injection(SSTI)](https://github.com/alejandro-pentest/Hacking-Web/blob/main/Server-Side%20Template%20Injection%20(SSTI)/Readme.md)
```python3
{{7*7}}
${{7*7}}
${7*7}
#{7*7}
<%= 7*7 %>
*{7*7}
```

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/a34977ac-863a-4ed1-975a-e764bfb6fd9e" width="500">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/af3b0e70-bec8-42e5-be78-afe976a87433" width="500">
