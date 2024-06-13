# Bypassing Blacklisted Characters.
Sometimes we may try to insert our payload but we can not do it because the server has a blacklist that don't let us to execute them. The blacklist can contain a list of operators commonly used in an
injection like '`;`' or '`&`', commands like '`whoami`' or '`id`', or even spaces '` `'.<br />

Besides injection operators and space characters, a very commonly blacklisted character is the slash `/` or backslash `\` character, as it is necessary to specify directories in Linux or Windows. 
We can utilize several techniques to produce any character we want while avoiding the use of blacklisted characters.

## Bypass overview.
- Sometimes will be enough just encoding some characters.
- In some cases we will need to use Environment VarioPATH.



## Linux 
### Bypass using Linux Environment Variables.
There are many techniques to bypass blacklists in Linux system, one of them is through Linux Environment Variables.<br />
If we check for example the `$PATH` variable we can see that we have a lot of characters that we can use:
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b32d0728-2e9b-420a-8ee1-f1535db17cec" width="700" alt="Sublime's custom image"/>
</p>

> We can choose any of the characters, for example, if we want to output the `/` we can do `${PATH:0:1}`, this command takes from the character 0 to character 1.


![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ee07dc7f-df2b-49ce-8729-df6e539af353)





### Space character.
The space character can be taken using the `${IFS}` path.

![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8691cd74-de16-4c58-8253-e10df07acd84)

> __Sometimes we can use `TAB` - > `%09` to bypass space.__


### ;

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/035e543c-df39-4061-bdab-d9112c79de82" width="600"><br />



![4](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/b8b53e60-1fee-42b4-84a0-51758d1e79ff)



> Using this technique we can obtain any character without the neeD to write it.




------------------------------------------------------------------

## Windows
### Bypass using Linux Environment Variables.
#### cms.
The same concept work on Windows as well. For example, to produce a slash in Windows Command Line (CMD), we can echo a Windows variable `echo %HOMEPATH%`.

![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/78e088ce-141b-4fcf-857e-1002c7dabcb4)

We will use the command `echo %HOMEPATH:~0,1%` to exctract the character `\`
:~0,1 is a substring operation that extracts the first character of the variable.
0 is the starting index.
1 is the number of characters to extract.


![7](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/f73f08af-e8b5-4860-a771-6b972e19144e)

#### PowerShell.
![8](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e487e00d-5ec6-4c24-953e-842bc92b1da7)


![9](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/15863a9c-fce5-4935-ba97-357b708122e3)

We can also use the Get-ChildItem Env: PowerShell command to print all environment variables and then pick one of them to produce a character we need. Try to be creative and find different commands 
to produce similar characters.
















