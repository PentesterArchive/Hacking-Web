# Command Injection Front-end Validation Bypass.
## Detecting Front-end Validation.
The target takes an IP adress and send a ping to it:

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/edcbbf86-d416-4f6b-9651-1f3063f6dd98" width="400" alt="Sublime's custom image"/>
</p>
We try to inject a command but we see the next validation: 


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/94a922d9-8820-4293-9b39-164751b5cbdf" width="500" alt="Sublime's custom image"/>
</p>




As we can see, the web application refused our input, as it seems only to accept input in an IP format. However, _from the look of the error message, it appears to be __originating from the front-end__ rather than the __back-end___.
> We can double-check this with the Developer Tools in the Network tab and then clicking on the Check button again:

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8319164e-048d-4c75-9cdd-54a2c041b7d9" width="500" alt="Sublime's custom image"/>
</p>

> When we click the `Check` button we don't see any data being send, that's the last proof to know that the validation is on the front-end.

This appears to be an attempt at preventing us from sending malicious payloads by only allowing user input in an IP format. However, it is very common for developers only to perform input validation on the front-end while not validating or sanitizing the input on the back-end.

However, as we will see, __front-end validations are usually not enough to prevent injections, as they can be very easily bypassed by sending custom HTTP requests directly to the back-end__.


## Bypassing Front-end validation.
The easiest method to customize the HTTP requests being sent to the back-end server is to use a web proxy that can __intercept the HTTP requests being sent by the application__. 
Then, we have to send a standard request from the web application with any IP (e.g. 127.0.0.1), on this step the request has being made, so we just need to customize it and send it back again from burpsuite.


<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0ad71c01-6158-4f24-9279-3368a41b5422" width="550">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/666330b0-0b5d-439d-9042-5c99d6010faa" width="450">





