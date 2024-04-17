# Log Poisoning.
In any server there are some files that register the logs. When we find a [Local File Inclusion](https://github.com/alejandro-pentest/Hacking-Web/tree/main/Local%20File%20Inclusion-Path%20traversal) vulnerability we can try to write some code in them and then try to execute it.

# Detection.
To detect if we are able to do a Log Poisoning we have to try to deploy any log file.
> Usually
