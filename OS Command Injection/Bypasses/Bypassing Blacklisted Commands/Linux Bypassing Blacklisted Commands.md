# Linux Bypassing Blacklisted Commands.
There are different methods when it comes to bypassing blacklisted commands. A command blacklist usually consists of a set of words, and if we can obfuscate our commands and make them look different, we may be able to bypass the filters.<br />
There are various methods of command obfuscation that vary in complexity, here we have a few basic techniques that may enable us to change the look of our command to bypass filters manually.
<br />
> A basic command blacklist filter in PHP would look like the following:
```php
$blacklist = ['whoami', 'cat', 'id', 'ipconfig'...etc];
foreach ($blacklist as $word) {
    if (strpos('$_POST['ip']', $word) !== false) {
        echo "Invalid input";
    }
}
```
As we can see, it is checking each word of the user input to see if it matches any of the blacklisted words. However, this code is looking for an exact match of the provided command, so if we send a slightly different command, it may not get blocked. Luckily, we can utilize various obfuscation techniques that will execute our command without using the exact command word.




## Linux & Windows
One very common and easy obfuscation technique is inserting certain characters within our command that are usually ignored by command shells like Bash or PowerShell and will execute the same command as if they were not there. Some of these characters are a single-quote `'` and a double-quote `"`, in addition to a few others.

The easiest to use are quotes, and __they work on both Linux and Windows servers__. For example, if we want to obfuscate the whoami command, we can insert single quotes between its characters (`w'h'o'am'i`). The same works with double-quotes as well `w"h"o"am"i` 
> The important things to remember are that _we __cannot mix types of quotes__ and the number of quotes must be __even__.

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/fd4a3853-c429-4f02-9e6a-5d56d992a83b" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/aae0aab9-4a1c-4c01-8e16-8246d1a9c7e4" width="400">




## Linux Only
We can insert a few other Linux-only characters in the middle of commands, and the bash shell would ignore them and execute the command. These characters include the backslash `\` and the positional parameter character `$@`. This works exactly as it did with the quotes, but in this case, the number of characters do not have to be even, and we can insert just one of them if we want to:
`who$@ami` `w\ho\am\i`
> If any of those characters are in the blacklist we can try to bypass it using the skills explained in [Command Injection - Character Bypass](https://github.com/alejandro-pentest/Hacking-Web/blob/main/OS%20Command%20Injection/Bypasses/Bypassing%20Blacklisted%20characters.md). The next is an example of combining both techniques:

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/2731f1c1-fe8d-41ac-9f4d-fe8c844da4fa" width="500">

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/74d1a5ea-8bf3-4bd6-99cb-b2fa2838b868" width="400">


### Linux Advanced Command Obfuscation.
In some instances, we may be dealing with advanced filtering solutions, like Web Application Firewalls (WAFs), and basic evasion techniques may not necessarily work. We can utilize more advanced techniques for such occasions, which make detecting the injected commands much less likely.<br />

#### Case Manipulation
One command obfuscation technique we can use is __case manipulation__, like inverting the character cases of a command (e.g. WHOAMI) or alternating between cases (e.g. WhOaMi). This usually works because a command blacklist may not check for different case variations of a single word, as Linux systems are case-sensitive.<br />
We have to get a bit creative and find a command that turns the command into an all-lowercase word. One working command we can use is the following:
```bash
(tr "[A-Z]" "[a-z]"<<<"WhOaMi")
```

![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8cfcc3bc-4298-43f9-8e0f-07c61693329a)

As we can see, the command did work, even though the word we provided was (WhOaMi). This command uses `tr` to __replace all upper-case characters with lower-case characters, which results in an all lower-case character command__.


--------------------

### Reversed Commands.
Another command obfuscation technique we will discuss is reversing commands and having a command template that switches them back and executes them in real-time. In this case, we will be writing `imaohw` instead of whoami to avoid triggering the blacklisted command.<br />
First, we'd have to get the reversed string of our command in our terminal, as follows: <br />
`echo 'whoami' | rev` <br />

Then, we can execute the original command by reversing it back in a sub-shell `($())`, as follows:<br />
```bash
$(rev<<<'imaohw')
```

![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/2d2215ed-3b20-4235-a00e-5ebf764adc54)


We see that even though the command does not contain the actual whoami word, it does work the same and provides the expected output.


> Tip: If you wanted to bypass a character filter with the above method, you'd have to reverse them as well, or include them when reversing the original command.


-----------------------------

### Encoded Commands
This new technique is helpful for commands containing filtered characters or characters that may be URL-decoded by the server. This may allow for the command to get messed up by the time it reaches the shell and eventually fails to execute. Instead of copying an existing command online, we will try to create our own unique obfuscation command this time. This way, it is much less likely to be denied by a filter or a WAF. The command we create will be unique to each case, depending on what characters are allowed and the level of security on the server.

We can utilize various encoding tools, like __base64__ or __xxd__ (for hex encoding).

#### Base64
First, we'll encode the payload we want to execute (which includes filtered characters):<br />
```echo -n 'cat /etc/passwd | grep 33' | base64```

> Now we can __create a command that will decode the encoded string in a sub-shell ($()), and then pass it to bash to be executed (i.e. bash<<<)__, as follows:
```bash
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```

As we can see, the above command executes the command perfectly. We did not include any filtered characters and avoided encoded characters that may lead the command to fail to execute.

> Tip: Note that we are using <<< to avoid using a pipe |, which is a filtered character.

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/3e27eacf-26d4-4d02-a6c8-12758a9a84d9" width="620">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/9efaf87c-2765-4b0f-a00b-fca7739146da" width="380">


> Even if some commands were filtered, like bash or base64, we could bypass that filter with the techniques we discussed in the previous section (e.g., character insertion), or use other alternatives like sh for command execution and openssl for b64 decoding, or xxd for hex decoding.

----------------------------

> In addition to the techniques we discussed, we can utilize numerous other methods, like wildcards, regex, output redirection, integer expansion, and many others. We can find some such techniques on PayloadsAllTheThings.







