# Server-side request forgery SSRF
Server-side request forgery is a web security ***vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location***.<br />
In a typical SSRF attack, ***the attacker might cause the server to make a connection to internal-only services*** within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could leak sensitive data, such as authorization credentials.

## How does it work?
In an SSRF attack against the server, ***the attacker causes the application to make an HTTP request back to the server that is hosting the application***, via its loopback network interface. This typically involves supplying a URL with a hostname like `127.0.0.1` or `localhost`.

--------------------------

## Basic SSRF against the local server.
Imagine a shopping application that lets the user view whether an item is in stock in a particular store. To provide the stock information, the application must query various back-end REST APIs. It does this by passing the URL to the relevant back-end API endpoint via a front-end HTTP request. When a user views the stock status for an item, their browser makes the following request:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```
This causes the server to make a request to the specified URL (`stockApi`), retrieve the stock status, and return this to the user.

In this example, an attacker can modify the request to specify a URL local to the server:


> ***An attacker can visit the /admin URL, but the administrative functionality is normally only accessible to authenticated users. This means an attacker won't see anything of interest. However, if the request to the /admin URL comes from the local machine, the normal access controls are bypassed. The application grants full access to the administrative functionality, because the request appears to originate from a trusted location.*** <br />

- Visiting /admin URL:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/43d1ea7d-620d-47c1-9b6d-33d7b1c8521e" width="1200">

- Checking the functionalities we can execute in /admin.
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e86000e7-52ea-48d0-841e-708d970dea4a" width="500">

- Executing an action:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/734968a3-c619-4cd5-9f50-9711f315f64f" width="600">

------------

## SSRF against another back-end system.
***In some cases, the application server is able to interact with back-end systems that are not directly reachable by users***. These systems often have non-routable private IP addresses. The back-end systems are normally protected by the network topology, so they often have a weaker security posture. In many cases, internal back-end systems contain sensitive functionality that can be accessed without authentication by anyone who is able to interact with the systems.
In this cases we can try to brute force the target IP on the vulnerable SSRF Url and check which one of the adresses gets a response.
- Brute forcing target IP to find back-end systems:
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/37975764-78c7-4534-89c4-89eb38d036e7" width="500">
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0b98d493-887f-40a6-884f-52867b4e6e17" width="500"><br />

- Visiting /admin URL:<br />
`<VulnerableSSRFUrl>=http://<NewIP>:<Port>/<adminPanel>`


----------------------------


## SSRF Serialization Bypass
### Serialized localhost.
In some cases we can see that the word localhost is blacklisted. We have different ways to refer `localhost`:
- `127.0.0.1`
- `127.1`
- `0`<br />
  .<br />
  .<br />
  .<br />






