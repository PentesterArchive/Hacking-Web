# XSS Cross-Site Scripting 
## What is XSS?
When a vulnerable web application does not properly sanitize user input, a malicious user can inject extra JavaScript code in an input field (e.g., comment/reply), so once another user views the same 
page, they unknowingly execute the malicious JavaScript code.</br>
XSS vulnerabilities are solely executed on the client-side and hence do not directly affect the back-end server. They can only affect the user executing the vulnerability. 
The direct impact of XSS vulnerabilities on the back-end server may be relatively low, but they are very commonly found in web applications.

## XSS Types
Existen tres tipos principales de vulnerabilidades XSS:

| Tipo                                   | Descripción                                                                                                                                                                                                                                                                                                      |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| XSS Almacenado __(Persistente)__       | El tipo más crítico de XSS, que ocurre cuando la entrada del usuario se almacena en la base de datos del back-end y luego se muestra al recuperarse (por ejemplo, publicaciones o comentarios)                                                                                                                   |
| XSS Reflejado __(No Persistente)__     | Ocurre cuando la entrada del usuario se muestra en la página después de ser procesada por el servidor backend (código fuente Ctrl+U), pero sin ser almacenada (por ejemplo, resultado de búsqueda o mensaje de error)                                                                                            |
| XSS Basado en DOM __(No Persistente)__ | Otro tipo de XSS No Persistente que ocurre cuando la entrada del usuario se muestra directamente en el navegador y se procesa completamente en el lado del cliente, sin llegar al servidor backend (código fuente Ctrl+U) (por ejemplo, a través de parámetros HTTP del lado del cliente o etiquetas de anclaje) |


