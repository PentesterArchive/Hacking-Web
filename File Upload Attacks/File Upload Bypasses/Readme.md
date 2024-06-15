# Bypasses.
-------------
## Index
1. [Blacklist.](#blacklist)<br />
2. [Whitelist.](#whitelist)<br />
    - [Double Extensions.](#double-extensions)<br />
    - [Reverse Double Extension](#reverse-double-extensions)
    - [Reverse Double Extension Attack. Example.](#reverse-double-extension-attack)
    - [Triple Extension.](#triple-extension)
3. [Character Injection.](#character-injection)
   

---------------

## Blacklist.
Sometimes we may deal with a blacklist that doesn't let us upload .php files. There are many different extensions that we can use to execute PHP files. So, the attack to bypass the blacklist consists of using brute force to find which extensions are permitted. If our target executes .php, we can try to brute force the following extension list [php - extensions list](https://github.com/alejandro-pentest/Hacking-Web/blob/main/File%20Upload%20Attacks/Extensions%20lists/php.txt).





----------------------------


## Whitelist.
A whitelist is generally more secure than a blacklist. __The web server would only allow the specified extensions__, and the list would not need to be comprehensive in covering uncommon extensions.
Still, there are different use cases for a blacklist and for a whitelist. A blacklist may be helpful in cases where the upload functionality needs to allow a wide variety of file types, while a whitelist is usually only used with upload functionalities where only a few file types are allowed. Both may also be used in tandem.

### Double Extensions
The code only tests whether the file name contains an image extension; a straightforward method of passing the regex test is through Double Extensions. For example, if the `.jpg` extension was allowed, we can add it in our uploaded file name and still end our filename with `.php` (e.g. `shell.jpg.php`), in which case we should be able to pass the whitelist test, while still uploading a PHP script that can execute PHP code.<br />
> However, this may not always work, as some web applications may use a strict regex that we can maybe bypass using the [Reverse Double Extension](#reverse-double-extension) bypass.

<br />

### Reverse Double Extension
```php
if (!preg_match('/^.*\.(jpg|jpeg|png|gif)$/', $fileName)) { ...SNIP... }
```
This pattern should _only consider the __final file extension___, as it uses `^.*\.` to match everything up to the last `.`, and then uses `$` at the end to only match extensions that end the file name. So, the [Double Extensions](double-extensions) bypass would not work. Nevertheless, some exploitation techniques may allow us to bypass this pattern, but most rely on misconfigurations or outdated systems.<br />
In such cases, we can try to upload a file that contains the above extensions and add them PHP code execution, even if it does not end with the PHP extension. For example, the file name `shell.php.jpg` should pass the earlier whitelist test as it ends with `.jpg`, and it would be able to execute PHP code due to a misconfiguration, as it contains `.php` in its name.

#### Reverse Double Extension Attack.
The first step to execute this attack is to brute force checking which extensions are permitted by the server, in other words we have to __check which extensions are whitelisted__. <br />
To do so we can use [Seclists - raft-large-extensions.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-extensions.txt). But if we know which type of files can
be uploaded, for example images, we can find a shorter list containing: png, jpg, jpeg, gif...

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/723a0263-2808-4792-9e6d-6a0d297ed818" width="1000">

> RESULT: `.jpg` files can be succesful uploaded to the server.<br />

Now we have to check __wich php extension type__ (in case that the target is using php) __are not blacklisted__.<br />
We can use [PHP extensions list](https://github.com/alejandro-pentest/Hacking-Web/blob/main/File%20Upload%20Attacks/Extensions%20lists/php.txt).
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/390d20e6-a0c8-47c5-973a-2e9fd725ebd5" width="900">

> RESULT: `.php` files are not allowed.


<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/9bf590ce-6fb0-4876-bb15-aaa4e50eaece" width="900">

> RESULT: The server knows that `.phar` is not an image and we must upload images. But `.phar` is not prohibited as `.php` was.

The next step is to mix both extensions and try to bypass the protections.<br />
I'm trying to bypass it with `.pht.jpg`, as `.pht` was avaliable too.<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/d422d70d-73c2-4fd2-a7fb-ea118eeb04c2" width="600">
> RESULT: File succesfuly uploaded, let's try to execute the php code on the server:
![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/04051e45-9060-4579-a9f8-51abc4fd57b9)

<br />

>__NOTE: Sometimes, even if the file has been successfully uploaded, we might not be able to execute it. In that case, we should try using other possible extensions.__

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/054867de-92d2-4258-8b87-37827bd0dee4" width="600">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ea328b5a-c0b7-41e9-ae55-e7dabda0499a" width="700">


--------------------------------------------------------------------------------------
### Triple Extension.
> Sometimes we maybe need to insert another extension to bypass the filter.
>We can use the following tool in this type of ocassions [GaboLC98/File-upload-vulnerability-abuse](https://github.com/GaboLC98/File-upload-vulnerability-abuse)

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/362a91e8-6f92-4a72-b1e5-7603d4e23f69" width="600">

-----------------------------------------------


## Character Injection
We can inject several characters before or after the final extension to cause the web application to misinterpret the filename and execute the uploaded file as a PHP script.

The following are some of the characters we may try injecting:
```php
    %20
    %0a
    %00
    %0d0a
    /
    .\
    .
    …
    :
```
Each character has a specific use case that may trick the web application to misinterpret the file extension. For example, (shell.php%00.jpg) works with PHP servers with version 5.X or earlier, as it causes the PHP web server to end the file name after the (%00), and store it as (shell.php), while still passing the whitelist. The same may be used with web applications hosted on a Windows server by injecting a colon (:) before the allowed file extension (e.g. shell.aspx:.jpg), which should also write the file as (shell.aspx). Similarly, each of the other characters has a use case that may allow us to upload a PHP script while bypassing the type validation test.<br />
> We can use [extensionCreator.sh](https://github.com/alejandro-pentest/Hacking-Web/blob/main/File%20Upload%20Attacks/Extensions%20lists/fUploadsInjection.sh) to create filenames using those characters and test if we can upload them.





