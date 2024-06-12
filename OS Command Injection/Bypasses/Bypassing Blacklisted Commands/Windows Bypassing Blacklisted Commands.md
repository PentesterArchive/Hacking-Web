# Windows Bypassing Blacklisted Commands.
There are different methods when it comes to bypassing blacklisted commands. A command blacklist usually consists of a set of words, and if we can obfuscate our commands and make them look different, we 
may be able to bypass the filters.<br />
There are various methods of command obfuscation that vary in complexity, here we have a few basic techniques that may enable us to change the look of our command to bypass filters manually.
<br />
> A basic command blacklist filter in PHP would look like the following:
```php
$blacklist = ['whoami', 'cat', 'dir', 'ifconfig'...etc];
foreach ($blacklist as $word) {
    if (strpos('$_POST['ip']', $word) !== false) {
        echo "Invalid input";
    }
}
```
As we can see, it is checking each word of the user input to see if it matches any of the blacklisted words. However, this code is looking for an exact match of the provided command, so if we send a slightly different command, it may not get blocked. 
Luckily, we can utilize various obfuscation techniques that will execute our command without using the exact command word.




## Linux & Windows
One very common and easy obfuscation technique is inserting certain characters within our command that are usually ignored by command shells like Bash or PowerShell and will execute the same command as if they were not there. Some of these characters are a single-quote `'` and a double-quote `"`, in addition to a few others.

The easiest to use are quotes, and __they work on both Linux and Windows servers__. For example, if we want to obfuscate the whoami command, we can insert single quotes between its characters (`w'h'o'am'i`). The same works with double-quotes as well `w"h"o"am"i` 
> The important things to remember are that _we __cannot mix types of quotes__ and the number of quotes must be __even__.

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/fd4a3853-c429-4f02-9e6a-5d56d992a83b" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/aae0aab9-4a1c-4c01-8e16-8246d1a9c7e4" width="400">


## Windows Only
There are also some Windows-only characters we can insert in the middle of commands that do not affect the outcome, like a caret `^` character, as we can see in the following example:

![12](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/d9e37a2c-e611-4383-8811-0f885a4ba46e)




