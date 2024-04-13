# Code Context Server Side Template Injection.
In a Code Context SSTI we have to deploy our payload in the code.

## Detection.
- We can see that our name is being displayed when we comment on the blog.<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0f5afb98-e63e-497f-a03c-8484dfd2a0f3" width="400"><br />
- When we check our configuration we can see the name function. ***We have to insert the code here to see if the server interprets it.***
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/cd8f44e5-5709-4ef6-86b6-be8f76491272" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/1cd0f288-375c-44f1-876b-96472abcd9e7" width="400">
<br />
- Trying to put an expresion after the `user.name` function.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/f44e54c3-80a7-463e-8f75-a78a66786668" width="600">
<br />

----------------------------------------

- After using the expression, we go back to the blog to check if the server interprets the `{{2*2}}` or not.
- **An error has ocurred, this error is because the function was not properly closed.** **Thanks to this error we can know that the templete being used is tornado.**
![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/491e8e88-55f7-4816-96eb-1f8de916476d)
- In this type of SSTI we must understand that we are trying to deploy our payload where a function was placed, so we will need to close it. The original comment code would be something like this:
```html
...
<section class="Comment">
  <p>
    <img src="/resources/images/avatarDefault.svg" class="avatar">
    {{user.name}} | {{comment.date}}
  </p>
  <p>
    {{comment.body}}
  </p>
</section>
...
```

-----------------------------------------------

- Correcting the mistake closing the function `user.name}`+ payload:
![7](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/567be3ab-f56c-4815-bd55-91dc874f67f3)
![8](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/adea82ab-c95a-4b2f-b8ee-ed81b846b6b3)

- Finally we can check some resources as [hacktricks-SSTI](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#detection) or [tornado SSTI](https://ajinabraham.com/blog/server-side-template-injection-in-tornado), where we can obtain the next succesful payload for tornado template: `{% import os %}{{ os.popen("whoami").read() }}`.
![9](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e4009d82-66aa-42f3-b35f-8048e0cb8437)
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/cefbf409-a669-416e-a215-8d2aef4f37e8" width="600">













