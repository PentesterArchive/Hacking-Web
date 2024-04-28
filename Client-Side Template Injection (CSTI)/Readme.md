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


## Exploitation.
Once we know that CSTI is being executed, we have to know which type of template is being used, after that we must search for a payload.

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/21d7cdc4-dbb3-460f-8d60-455d67205bc4" width="600">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/13375fb9-3bfa-4999-86eb-9031659c65bd" width="600">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ca573ff5-b4f8-4b70-9e83-96d3b61c9528" width="600">


