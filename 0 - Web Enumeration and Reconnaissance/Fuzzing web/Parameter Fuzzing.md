# Parameter Fuzzing.
Sometimes we guess that a parameter such as `file`, `filename`, `cmd` or something similar could be hidden. In this case, we can fuzz the parameter.

- Parameters list:![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/c59d8f6d-bded-4d83-9f7a-79631e82f4a8)
- [Parameters list 2](https://raw.githubusercontent.com/whiteknight7/wordlist/refs/heads/main/fuzz-lfi-params-list.txt)

## FFUF.
![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/cde6e5f3-539f-4fea-82c8-3567eeef1656)<br />

![2](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/edeb0a26-cfd6-4a1f-b493-2ad4a5c9f7ec)

- In this case we saw a lot of outputs, so we are going to `match code` with `-mc`. <br />
`ffuf -w <Wordlists> -u <url\?FUZZ\=<command>> -mc 200`

![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/01ff5781-fbc9-4ac5-97de-b1382a43f78a)<br />

![4](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/059ce045-d655-485c-bf75-ffce40444208)

