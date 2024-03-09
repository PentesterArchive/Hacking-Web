# Server-side request forgery SSRF!
Server-side request forgery is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location.
In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could leak sensitive data, such as authorization credentials.

## How does it work?
In an SSRF attack against the server, the attacker causes the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface. This typically involves supplying a URL with a hostname like 127.0.0.1 (a reserved IP address that points to the loopback adapter) or localhost (a commonly used name for the same adapter).
For example, imagine a shopping application that lets the user view whether an item is in stock in a particular store. To provide the stock information, the application must query various back-end REST APIs. It does this by passing the URL to the relevant back-end API endpoint via a front-end HTTP request. When a user views the stock status for an item, their browser makes the following request:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```
This causes the server to make a request to the specified URL, retrieve the stock status, and return this to the user.

In this example, an attacker can modify the request to specify a URL local to the server:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
```

## Gaining administrative functionality
The server fetches the contents of the /admin URL and returns it to the user.

***An attacker can visit the /admin URL, but the administrative functionality is normally only accessible to authenticated users. This means an attacker won't see anything of interest. However, if the request to the /admin URL comes from the local machine, the normal access controls are bypassed. The application grants full access to the administrative functionality, because the request appears to originate from a trusted location.*** <br />

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8241cb7d-6d2d-4a76-a78d-a1b278ea23e1" width=900 height=150>


![5](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/8241cb7d-6d2d-4a76-a78d-a1b278ea23e1|50)

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/58db7c45-9f09-4d6a-8846-bdc0f8d033f7" width=900 height=150>


![6](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/58db7c45-9f09-4d6a-8846-bdc0f8d033f7)

![7](https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/aaf6569d-7f68-4fad-81bb-bad93440307c)






