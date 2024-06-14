# Type Filters
Only testing the file extension is not enough to prevent file upload attacks.
This is why many modern web servers and web applications also test the content of the uploaded file to ensure it matches the specified type. While extension filters may accept several extensions, content
filters usually specify a single category (e.g., images, videos, documents), which is why they do not typically use blacklists or whitelists. This is because web servers provide functions to check for 
the file content type, and it usually falls under a specific category.

There are two common methods for validating the file content: Content-Type Header or File Content. Let's see how we can identify each filter and how to bypass both of them.

## Content-Type Header.
We see that we get a message saying Only images are allowed. The error message persists, and our file fails to upload even if we try some of the tricks we learned in the previous sections. 
If we change the file name to shell.jpg.phtml or shell.php.jpg, or even if we use shell.jpg with a web shell content, our upload will fail. As the file extension does not affect the error message, 
the web application must be testing the file content for type validation. As mentioned earlier, this can be either in the Content-Type Header or the File Content.

The following is an example of how a PHP web application tests the Content-Type header to validate the file type:


```php
$type = $_FILES['uploadFile']['type'];

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

The code sets the `$type` variable from the uploaded file's Content-Type header. Our browsers automatically set the Content-Type header when selecting a file through the file selector dialog,
usually derived from the file extension. However, since our browsers set this, this operation is a client-side operation, and we can manipulate it to change the perceived file type and potentially 
bypass the type filter.

We may start by fuzzing the Content-Type header with [SecLists Content-Type Wordlist](https://github.com/danielmiessler/SecLists/blob/master/Miscellaneous/Web/content-type.txt) through Burp Intruder,
to see which types are allowed. However, the message tells us that only images are allowed, so we can limit our scan to image types, which reduces the wordlist to 45 types only 
(compared to around 700 originally). We can do so as follows:

```bash
wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Miscellaneous/web/content-type.txt
cat content-type.txt | grep 'image/' > image-content-types.txt
```







This time we get File successfully uploaded, and if we visit our file, we see that it was successfully uploaded: 











Exercise: Try to run the above scan to find what Content-Types are allowed.

For the sake of simplicity, let's just pick an image type (e.g. image/jpg), then intercept our upload request and change the Content-Type header to it: 


