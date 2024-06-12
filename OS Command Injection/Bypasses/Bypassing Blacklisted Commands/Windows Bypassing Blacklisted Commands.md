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

## Case Manipulation
One command obfuscation technique we can use is case manipulation, like inverting the character cases of a command (e.g. WHOAMI) or alternating between cases (e.g. WhOaMi). This usually works because a command blacklist may not check for different case variations of a single word.<br />
If we are dealing with a Windows server, we can change the casing of the characters of the command and send it. In Windows, commands for PowerShell and CMD are case-insensitive, meaning they will execute the command regardless of what case it is written in:

![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/38e1c0b4-e5de-4b29-8060-fa055dcbd10e)





## Reversed Commands
First, we'd have to get the reversed string of our command in our terminal, as follows:<br />
`echo 'whoami' | rev`<br />

We see that even though the command does not contain the actual whoami word, it does work the same and provides the expected output.<br />
`"whoami"[-1..-20] -join ''`<br />

![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7f8f3a11-b00c-46f6-a841-8210bbaf6758)

We can now use the below command to execute a reversed string with a PowerShell sub-shell (iex "$()"), as follows:<br />
```bash
iex "$('imaohw'[-1..-20] -join '')"
```

![7](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e340d79b-a974-4fd5-95ae-44ea04bf3758)




----------------------
## Encoded commands.
This new technique is helpful for commands containing filtered characters or characters that may be URL-decoded by the server. This may allow for the command to get messed up by the time it reaches the shell and eventually fails to execute. Instead of copying an existing command online, we will try to create our own unique obfuscation command this time. This way, it is much less likely to be denied by a filter or a WAF. The command we create will be unique to each case, depending on what characters are allowed and the level of security on the server.<br />
We can utilize various encoding tools, like base64 or xxd (for hex encoding).

### Base64
First, we need to base64 encode our string, as follows: <br />
```powershell
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))
```
> __RESULT:__ `dwBoAG8AYQBtAGkA`



![8](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ba336585-ce0d-4bf6-8eba-657c7e741bab)



We may also achieve the same thing on Linux, but we would have to convert the string from utf-8 to utf-16 before we base64 it, as follows:
```bash
echo -n whoami | iconv -f utf-8 -t utf-16le | base64
``` 
> __RESULT:__ `dwBoAG8AYQBtAGkA`

Finally, we can decode the `b64` string and execute it with a PowerShell sub-shell (`iex "$()"`), as follows:
```powershell
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"
```
![9](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/38a9f3d4-d99f-40db-b53a-4b396d95b36a)



As we can see, we can get creative with Bash or PowerShell and create new bypassing and obfuscation methods that have not been used before, and hence are very likely to bypass filters and WAFs. Several tools can help us automatically obfuscate our commands.

> In addition to the techniques we discussed, we can utilize numerous other methods, like wildcards, regex, output redirection, integer expansion, and many others. We can find some such techniques on PayloadsAllTheThings.
