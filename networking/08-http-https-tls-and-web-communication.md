# 08 - HTTP, HTTPS, TLS and Web Communication

## Table of Contents

* [What Happens When You Open a Website?](#what-happens-when-you-open-a-website)
* [What is HTTP?](#what-is-http)
* [HTTP Client and Server](#http-client-and-server)
* [HTTP Request and Response](#http-request-and-response)
* [HTTP Request Structure](#http-request-structure)
* [HTTP Methods](#http-methods)
* [HTTP Response Structure](#http-response-structure)
* [HTTP Status Codes](#http-status-codes)
* [HTTP Headers](#http-headers)
* [HTTP Body](#http-body)
* [HTTP is Stateless](#http-is-stateless)
* [Cookies](#cookies)
* [Sessions](#sessions)
* [What is HTTPS?](#what-is-https)
* [What is TLS?](#what-is-tls)
* [Why TLS is Needed](#why-tls-is-needed)
* [Encryption Basics](#encryption-basics)
* [Symmetric Encryption](#symmetric-encryption)
* [Asymmetric Encryption](#asymmetric-encryption)
* [Digital Certificates](#digital-certificates)
* [Certificate Authority](#certificate-authority)
* [TLS Handshake](#tls-handshake)
* [Server Name Indication](#server-name-indication)
* [HTTP vs HTTPS](#http-vs-https)
* [Proxy Servers](#proxy-servers)
* [Forward Proxy](#forward-proxy)
* [Reverse Proxy](#reverse-proxy)
* [Complete Web Communication Flow](#complete-web-communication-flow)
* [Cybersecurity and SOC Relevance](#cybersecurity-and-soc-relevance)
* [Useful Commands](#useful-commands)
* [Practical Labs](#practical-labs)
* [What I Learned](#what-i-learned)

---

# What Happens When You Open a Website?

Suppose you open a browser and type:

```text
https://example.com
```

Many networking processes happen before the website appears.

A simplified process is:

```text
User Enters URL
      ↓
Browser Checks Cache
      ↓
DNS Resolution
      ↓
Destination IP Found
      ↓
Routing Decision
      ↓
ARP Finds Gateway MAC
      ↓
TCP Connection
      ↓
TLS Handshake
      ↓
HTTP Request
      ↓
HTTP Response
      ↓
Browser Renders Website
```

This process connects many networking concepts.

From previous sections:

```text
DNS
Domain → IP
```

```text
Routing
Find Network Path
```

```text
ARP
IP → MAC
```

```text
TCP
Reliable Connection
```

Now:

```text
TLS
Encrypted Communication
```

and:

```text
HTTP
Web Communication
```

---

# What is HTTP?

**HTTP stands for Hypertext Transfer Protocol.**

HTTP is an application-layer protocol used for communication between web clients and web servers.

HTTP commonly works at:

```text
OSI Layer 7
Application Layer
```

Example:

```text
Browser
   |
   | HTTP
   ↓
Web Server
```

The browser sends an HTTP request.

The server sends an HTTP response.

```text
Client → HTTP Request → Server

Client ← HTTP Response ← Server
```

HTTP commonly uses:

```text
TCP Port 80
```

Example:

```text
Client
192.168.1.10:52000

Server
93.184.x.x:80
```

Connection:

```text
192.168.1.10:52000
        ↓
93.184.x.x:80
```

---

# HTTP Client and Server

HTTP uses a client-server model.

The client requests resources.

The server provides resources.

Example clients:

```text
Web Browser
Mobile Application
Python Script
API Client
```

Example servers:

```text
Apache
Nginx
IIS
Application Server
```

Simple communication:

```text
Client
  |
  | Request
  ↓
Server
  |
  | Response
  ↓
Client
```

Suppose the browser requests:

```text
/index.html
```

The server may return:

```html
<h1>Hello World</h1>
```

The browser displays the page.

---

# HTTP Request and Response

HTTP communication mainly uses:

```text
Request
Response
```

Example:

```text
Browser → Request → Server

Browser ← Response ← Server
```

A request asks for a resource or performs an action.

A response contains the server's result.

Example request:

```http
GET /index.html HTTP/1.1
Host: example.com
```

Example response:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<h1>Hello</h1>
```

---

# HTTP Request Structure

An HTTP request commonly contains:

```text
Request Line
Headers
Blank Line
Body
```

Structure:

```text
Request Line
Headers

Body
```

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=admin&password=test
```

The sections are:

```text
POST /login HTTP/1.1
        ↓
Request Line
```

```text
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29
        ↓
Headers
```

```text
username=admin&password=test
        ↓
Body
```

---

## HTTP Request Line

Example:

```http
GET /index.html HTTP/1.1
```

The request line contains:

```text
HTTP Method
Resource Path
HTTP Version
```

Breaking it down:

```text
GET
 ↓
HTTP Method
```

```text
/index.html
 ↓
Requested Resource
```

```text
HTTP/1.1
 ↓
HTTP Version
```

---

# HTTP Methods

HTTP methods describe the action a client wants to perform.

Common methods include:

| Method  | Purpose                |
| ------- | ---------------------- |
| GET     | Retrieve data          |
| POST    | Send or create data    |
| PUT     | Replace or update data |
| PATCH   | Partially update data  |
| DELETE  | Delete data            |
| HEAD    | Retrieve headers       |
| OPTIONS | Show supported methods |

---

## GET

GET retrieves data from a server.

Example:

```http
GET /products HTTP/1.1
Host: example.com
```

Simple meaning:

> Give me the products page.

Another example:

```http
GET /users/10 HTTP/1.1
```

Meaning:

> Give me information about user 10.

GET requests may contain query parameters.

Example:

```text
/search?q=networking
```

The query parameter is:

```text
q=networking
```

Complete example:

```http
GET /search?q=networking HTTP/1.1
Host: example.com
```

---

## POST

POST sends data to a server.

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=user&password=test
```

POST is commonly used for:

```text
Login Forms
Registration Forms
File Uploads
API Requests
Creating Data
```

Simple meaning:

> I am sending data to the server.

---

## PUT

PUT commonly replaces or updates a complete resource.

Example:

```http
PUT /users/10 HTTP/1.1
Content-Type: application/json

{
  "name": "Alex",
  "role": "admin"
}
```

Simple meaning:

> Replace or update this resource with the provided data.

---

## PATCH

PATCH partially updates a resource.

Example:

```http
PATCH /users/10 HTTP/1.1
Content-Type: application/json

{
  "role": "admin"
}
```

Only the role may be updated.

Simple difference:

```text
PUT
Replace Complete Resource
```

```text
PATCH
Update Part of Resource
```

---

## DELETE

DELETE requests the removal of a resource.

Example:

```http
DELETE /users/10 HTTP/1.1
```

Simple meaning:

> Delete user 10.

---

## HEAD

HEAD is similar to GET.

However, the server normally returns headers without the response body.

Example:

```http
HEAD /index.html HTTP/1.1
```

Useful for checking:

```text
Resource Availability
Content Type
Content Length
Caching Information
```

---

## OPTIONS

OPTIONS asks the server which HTTP methods or communication options are supported.

Example:

```http
OPTIONS /api/users HTTP/1.1
```

A response may contain:

```http
Allow: GET, POST, OPTIONS
```

Security analysts may inspect supported HTTP methods.

Unexpected methods may require investigation.

---

# HTTP Response Structure

An HTTP response commonly contains:

```text
Status Line
Headers
Blank Line
Body
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 14

<h1>Hello</h1>
```

The status line is:

```text
HTTP/1.1 200 OK
```

It contains:

```text
HTTP Version
Status Code
Status Message
```

---

# HTTP Status Codes

HTTP status codes describe the result of an HTTP request.

Status codes are divided into categories.

| Range   | Category      |
| ------- | ------------- |
| 100-199 | Informational |
| 200-299 | Success       |
| 300-399 | Redirection   |
| 400-499 | Client Error  |
| 500-599 | Server Error  |

Simple memory:

```text
1xx → Information

2xx → Success

3xx → Redirect

4xx → Client Error

5xx → Server Error
```

---

## 200 OK

Example:

```text
200 OK
```

Meaning:

> The request was successful.

Example:

```http
HTTP/1.1 200 OK
```

---

## 201 Created

Example:

```text
201 Created
```

Meaning:

> A new resource was successfully created.

Common with:

```text
POST Requests
API Requests
```

---

## 301 Moved Permanently

Example:

```text
301 Moved Permanently
```

Meaning:

> The resource has permanently moved to another location.

Example:

```text
http://example.com
        ↓
https://example.com
```

---

## 302 Found

A `302` response commonly indicates a temporary redirect.

Example:

```text
/login
   ↓
/dashboard
```

The browser follows the redirect location.

---

## 304 Not Modified

Example:

```text
304 Not Modified
```

Meaning:

> The cached resource is still valid.

The browser may use its cached copy.

This reduces data transfer.

---

## 400 Bad Request

Example:

```text
400 Bad Request
```

Meaning:

> The server could not understand the request.

Possible reasons:

```text
Invalid Request Syntax
Malformed Data
Invalid Parameters
```

---

## 401 Unauthorized

Example:

```text
401 Unauthorized
```

Meaning:

> Authentication is required or authentication failed.

Simple example:

```text
Client → Protected Resource

Server → 401 Unauthorized
```

---

## 403 Forbidden

Example:

```text
403 Forbidden
```

Meaning:

> The server understood the request but refuses access.

Simple difference:

```text
401
Authentication Problem
```

```text
403
Access Not Allowed
```

---

## 404 Not Found

Example:

```text
404 Not Found
```

Meaning:

> The requested resource does not exist.

Example:

```text
/secret-page
```

If the resource is unavailable:

```text
404 Not Found
```

---

## 405 Method Not Allowed

Example:

```text
405 Method Not Allowed
```

Meaning:

> The HTTP method is not allowed for the resource.

Example:

```http
DELETE /users HTTP/1.1
```

If DELETE is not supported:

```text
405 Method Not Allowed
```

---

## 429 Too Many Requests

Example:

```text
429 Too Many Requests
```

Meaning:

> The client sent too many requests.

This may occur because of:

```text
Rate Limiting
API Limits
Brute Force Protection
```

SOC analysts may investigate repeated `429` responses.

---

## 500 Internal Server Error

Example:

```text
500 Internal Server Error
```

Meaning:

> The server encountered an internal problem.

Possible causes:

```text
Application Error
Code Failure
Configuration Problem
Database Problem
```

---

## 502 Bad Gateway

Example:

```text
502 Bad Gateway
```

This commonly occurs when a proxy or gateway receives an invalid response from another server.

Example:

```text
Client
  ↓
Reverse Proxy
  ↓
Backend Server
```

If the backend responds incorrectly:

```text
502 Bad Gateway
```

---

## 503 Service Unavailable

Example:

```text
503 Service Unavailable
```

Meaning:

> The server is temporarily unable to handle the request.

Possible reasons:

```text
Server Overload
Maintenance
Backend Failure
```

---

## 504 Gateway Timeout

Example:

```text
504 Gateway Timeout
```

A gateway or proxy did not receive a response from an upstream server in time.

Example:

```text
Client
  ↓
Proxy
  ↓
Backend Server
      X
No Response
```

Result:

```text
504 Gateway Timeout
```

---

# HTTP Headers

HTTP headers contain additional information about a request or response.

Format:

```text
Header-Name: Header-Value
```

Example:

```http
Host: example.com
```

Another example:

```http
User-Agent: Mozilla/5.0
```

Common headers include:

```text
Host
User-Agent
Content-Type
Content-Length
Authorization
Cookie
Set-Cookie
Referer
Origin
Server
Location
```

---

## Host Header

Example:

```http
Host: example.com
```

The Host header identifies the requested domain.

A single server may host multiple websites.

Example:

```text
Server IP:
192.168.1.10
```

Websites:

```text
example.com

shop.example.com

blog.example.com
```

The Host header helps the server identify which website the client wants.

---

## User-Agent Header

Example:

```http
User-Agent: Mozilla/5.0
```

The User-Agent may provide information about the client.

Possible information:

```text
Browser
Operating System
Application
HTTP Library
```

Example:

```text
Mozilla/5.0
```

may indicate a browser.

Example:

```text
python-requests
```

may indicate a Python script using an HTTP library.

User-Agent values can be modified.

Therefore:

> A User-Agent should not be trusted as proof of the actual application.

---

## Content-Type Header

Content-Type describes the format of the body.

Examples:

```text
text/html
```

```text
application/json
```

```text
application/xml
```

```text
image/png
```

Example:

```http
Content-Type: application/json
```

The body may contain:

```json
{
  "name": "Alex"
}
```

---

## Content-Length Header

Example:

```http
Content-Length: 100
```

It indicates the size of the message body in bytes.

---

## Authorization Header

The Authorization header may contain authentication information.

Example:

```http
Authorization: Bearer TOKEN
```

It is commonly used by APIs.

Sensitive authentication data should be protected using HTTPS.

---

## Cookie Header

The browser may send cookies using:

```http
Cookie: session_id=abc123
```

The server uses the cookie to identify state or session information.

---

## Set-Cookie Header

A server sends cookies using:

```http
Set-Cookie: session_id=abc123
```

The browser may store the cookie.

Future requests may contain:

```http
Cookie: session_id=abc123
```

---

## Location Header

The Location header is commonly used for redirects.

Example:

```http
HTTP/1.1 302 Found
Location: /dashboard
```

The browser may request:

```text
/dashboard
```

---

# HTTP Body

The HTTP body contains the actual message data.

Example HTML body:

```html
<h1>Hello World</h1>
```

JSON body:

```json
{
  "username": "alex",
  "role": "user"
}
```

Form body:

```text
username=alex&password=test
```

A GET request often does not require a body.

POST, PUT, and PATCH requests commonly contain bodies.

---

# HTTP is Stateless

HTTP is a stateless protocol.

This means:

> Each HTTP request is treated as an independent request.

Example:

```text
Request 1
GET /login
```

Then:

```text
Request 2
GET /dashboard
```

HTTP itself does not automatically remember:

```text
Who the user is
Whether the user logged in
Previous requests
Shopping cart contents
```

Web applications use mechanisms such as:

```text
Cookies
Sessions
Tokens
```

to maintain state.

---

# Cookies

A cookie is small data stored by the browser.

Example:

```text
session_id=abc123
```

The server sends:

```http
Set-Cookie: session_id=abc123
```

The browser stores the cookie.

Later:

```http
Cookie: session_id=abc123
```

Communication:

```text
Server
  |
  | Set-Cookie
  ↓
Browser
  |
  | Stores Cookie
  ↓
Future Request
  |
  | Cookie
  ↓
Server
```

Cookies may be used for:

```text
Sessions
Preferences
Tracking
Authentication State
```

---

## Important Cookie Attributes

Common cookie attributes include:

```text
Secure
HttpOnly
SameSite
Expires
Max-Age
Domain
Path
```

---

## Secure Cookie

Example:

```http
Set-Cookie: session_id=abc123; Secure
```

The `Secure` attribute tells the browser to send the cookie only over secure HTTPS connections.

---

## HttpOnly Cookie

Example:

```http
Set-Cookie: session_id=abc123; HttpOnly
```

`HttpOnly` restricts JavaScript access to the cookie.

This can help reduce the risk of cookie theft through some Cross-Site Scripting attacks.

---

## SameSite Cookie

The `SameSite` attribute controls when cookies are sent with cross-site requests.

Common values include:

```text
Strict
Lax
None
```

It can help reduce some Cross-Site Request Forgery risks.

---

# Sessions

A session allows a server to remember a user across multiple requests.

Example:

```text
User Logs In
     ↓
Server Creates Session
     ↓
Session ID Generated
     ↓
Session ID Sent as Cookie
```

Example:

```text
session_id=abc123
```

The server may store:

```text
abc123 → User Alex
```

Future request:

```http
GET /dashboard HTTP/1.1
Cookie: session_id=abc123
```

The server checks:

```text
abc123
   ↓
Session Database
   ↓
User Alex
```

The server now knows which user made the request.

Simple flow:

```text
Cookie
   ↓
Session ID
   ↓
Server Session
   ↓
User Information
```

---

# What is HTTPS?

**HTTPS stands for Hypertext Transfer Protocol Secure.**

HTTPS is HTTP communication protected using TLS.

Simple formula:

```text
HTTPS = HTTP + TLS
```

HTTP commonly uses:

```text
TCP Port 80
```

HTTPS commonly uses:

```text
TCP Port 443
```

HTTP:

```text
Client → HTTP → Server
```

HTTPS:

```text
Client → Encrypted HTTP → Server
```

The HTTP data is protected inside a TLS connection.

---

# What is TLS?

**TLS stands for Transport Layer Security.**

TLS protects network communication.

TLS provides:

```text
Confidentiality
Integrity
Authentication
```

Simple explanation:

```text
Confidentiality
Attackers should not read the data
```

```text
Integrity
Attackers should not modify the data unnoticed
```

```text
Authentication
Client can verify the server identity
```

TLS is used by:

```text
HTTPS
Secure Email Protocols
APIs
VPN Technologies
Other Network Applications
```

---

# Why TLS is Needed

Suppose a user sends HTTP data.

```http
POST /login HTTP/1.1

username=alex&password=secret123
```

Without encryption:

```text
Client
  ↓
username=alex
password=secret123
  ↓
Server
```

A person able to capture the traffic may potentially read the data.

With TLS:

```text
Client
  ↓
Encrypted Data
  ↓
Server
```

Captured traffic may appear unreadable without the required cryptographic keys.

Therefore, TLS protects data in transit.

---

# Encryption Basics

Encryption converts readable data into an unreadable form.

Readable data is called:

```text
Plaintext
```

Encrypted data is called:

```text
Ciphertext
```

Process:

```text
Plaintext
   ↓
Encryption
   ↓
Ciphertext
```

Example:

```text
Hello
  ↓
Encryption
  ↓
X7#2kP9
```

Decryption reverses the process.

```text
Ciphertext
   ↓
Decryption
   ↓
Plaintext
```

Encryption uses:

```text
Algorithms
Keys
```

---

# Symmetric Encryption

Symmetric encryption uses the same secret key for encryption and decryption.

Example:

```text
Secret Key
    ↓
Encrypt Data
```

The receiver uses the same secret key.

```text
Secret Key
    ↓
Decrypt Data
```

Diagram:

```text
Sender
  |
  | Secret Key
  ↓
Encrypted Data
  |
  ↓
Receiver
  |
  | Same Secret Key
  ↓
Decrypted Data
```

Symmetric encryption is generally fast.

It is useful for encrypting large amounts of data.

Examples of symmetric encryption algorithms include:

```text
AES
ChaCha20
```

TLS commonly uses symmetric encryption after establishing secure session keys.

---

# Asymmetric Encryption

Asymmetric encryption uses two related keys.

```text
Public Key
Private Key
```

The public key can be shared.

The private key should remain secret.

Simple concept:

```text
Public Key
Can Be Shared
```

```text
Private Key
Must Be Protected
```

The keys are mathematically related.

Asymmetric cryptography is used in TLS for mechanisms such as:

```text
Authentication
Digital Signatures
Key Exchange
```

depending on the TLS version and cipher suite.

Examples include technologies based on:

```text
RSA
Elliptic Curve Cryptography
```

---

## Symmetric vs Asymmetric Encryption

| Symmetric                     | Asymmetric                               |
| ----------------------------- | ---------------------------------------- |
| One secret key                | Public and private key                   |
| Generally faster              | Generally slower                         |
| Good for bulk data encryption | Good for authentication and key exchange |
| Key must be shared securely   | Public key can be shared                 |

TLS combines cryptographic techniques.

Simple idea:

```text
Asymmetric Cryptography
        ↓
Establish Trust / Exchange Key Material
        ↓
Symmetric Encryption
        ↓
Encrypt Application Data
```

---

# Digital Certificates

A digital certificate helps prove the identity of a server.

Suppose you connect to:

```text
https://example.com
```

The server sends a certificate.

The certificate may contain:

```text
Domain Name
Public Key
Issuer
Validity Period
Digital Signature
Certificate Information
```

Example:

```text
Domain:
example.com

Issuer:
Certificate Authority

Valid From:
Date A

Valid Until:
Date B
```

The browser checks the certificate.

Questions include:

```text
Is the certificate valid?

Is it expired?

Does the domain match?

Is the issuer trusted?

Is the signature valid?
```

If the checks succeed, the browser can trust the server identity according to the certificate chain.

---

# Certificate Authority

A Certificate Authority is commonly called a:

```text
CA
```

A CA issues and signs digital certificates.

The CA acts as a trusted third party.

Simple model:

```text
Browser Trusts CA
       ↓
CA Signs Server Certificate
       ↓
Browser Trusts Certificate
```

Examples of certificate authorities include organizations that issue publicly trusted TLS certificates.

Browsers and operating systems maintain trusted root certificate stores.

Example chain:

```text
Root CA
   ↓
Intermediate CA
   ↓
Server Certificate
```

This is called a:

```text
Certificate Chain
```

---

## Self-Signed Certificate

A self-signed certificate is signed using its own private key instead of being signed through a trusted public CA chain.

Example:

```text
Server
  ↓
Signs Own Certificate
```

Browsers may show a warning because the certificate is not trusted by the browser's normal public trust store.

Self-signed certificates may be used in:

```text
Labs
Internal Networks
Testing Environments
Development
```

Trust must be managed carefully.

---

# TLS Handshake

The TLS handshake establishes secure communication.

Modern TLS versions may use different handshake details.

A simplified TLS handshake is:

```text
Client
  |
  | Client Hello
  ↓
Server
  |
  | Server Hello
  | Certificate
  ↓
Client
  |
  | Certificate Validation
  | Key Agreement
  ↓
Server
  |
  | Key Agreement
  ↓
Encrypted Communication
```

---

## Step 1 - Client Hello

The client sends a:

```text
Client Hello
```

It may contain information such as:

```text
Supported TLS Versions
Supported Cipher Suites
Client Random Data
Extensions
Server Name
```

Simple meaning:

> These are the TLS options I support.

---

## Step 2 - Server Hello

The server sends:

```text
Server Hello
```

The server selects supported TLS parameters.

It may include:

```text
Selected TLS Version
Selected Cipher Suite
Server Random Data
TLS Extensions
```

The server also provides certificate information as required by the handshake.

---

## Step 3 - Certificate Validation

The client validates the server certificate.

The browser checks:

```text
Domain Name
Certificate Validity
Certificate Signature
Certificate Chain
Trusted CA
```

If certificate validation fails, the browser may display a security warning.

---

## Step 4 - Key Agreement

The client and server perform a cryptographic key agreement.

The goal is to create shared secret key material.

Simplified:

```text
Client
   ↓
Cryptographic Key Agreement
   ↓
Server
```

Both sides derive session keys.

These session keys are used for encrypted communication.

---

## Step 5 - Encrypted Communication

After the handshake:

```text
HTTP Request
      ↓
TLS Encryption
      ↓
Encrypted Network Traffic
```

The server decrypts the data.

```text
Encrypted Data
      ↓
TLS Decryption
      ↓
HTTP Request
```

The response is also encrypted.

Simple formula:

```text
HTTP
  ↓
TLS
  ↓
TCP
  ↓
IP
  ↓
Ethernet / Wi-Fi
```

---

# Server Name Indication

SNI stands for:

```text
Server Name Indication
```

SNI is a TLS extension.

It allows a client to indicate which hostname it wants to connect to.

Example server:

```text
IP Address:
192.168.1.10
```

Websites:

```text
example.com

shop.com

blog.com
```

The client wants:

```text
example.com
```

The TLS Client Hello can include:

```text
Server Name:
example.com
```

The server can select the correct certificate.

Simple flow:

```text
Client Hello
     ↓
SNI = example.com
     ↓
Server
     ↓
Correct Certificate
```

SNI can be useful during network traffic analysis because the requested hostname may sometimes be visible depending on the TLS technology and network configuration.

Newer privacy technologies can change what metadata is visible.

---

# HTTP vs HTTPS

| HTTP                            | HTTPS                         |
| ------------------------------- | ----------------------------- |
| Hypertext Transfer Protocol     | HTTP protected by TLS         |
| Commonly port 80                | Commonly port 443             |
| Data may be readable in transit | Application data is encrypted |
| No TLS certificate required     | Uses TLS certificates         |
| No TLS handshake                | Uses TLS handshake            |

Simple rule:

```text
HTTP
Web Communication
```

```text
HTTPS
Encrypted Web Communication
```

Remember:

```text
HTTPS = HTTP + TLS
```

---

# Proxy Servers

A proxy is a server that sits between systems.

Example:

```text
Client
  ↓
Proxy
  ↓
Server
```

The client communicates with the proxy.

The proxy communicates with another server.

Common proxy types include:

```text
Forward Proxy
Reverse Proxy
```

---

# Forward Proxy

A forward proxy represents clients.

Example:

```text
Client A ─┐
Client B ─┼→ Forward Proxy → Internet
Client C ─┘
```

The Internet server may see the proxy connection instead of a direct connection from the internal client.

Forward proxies may be used for:

```text
Web Filtering
Access Control
Traffic Logging
Caching
Privacy
Corporate Internet Control
```

Example:

```text
Employee
   ↓
Corporate Proxy
   ↓
Website
```

The proxy may log:

```text
Source User
Source IP
Requested Domain
URL
Timestamp
Action
```

These logs can be important for SOC investigations.

---

# Reverse Proxy

A reverse proxy represents servers.

Example:

```text
Internet Client
      ↓
Reverse Proxy
      ↓
Web Server A
Web Server B
Web Server C
```

The client connects to the reverse proxy.

The reverse proxy forwards the request to a backend server.

Reverse proxies may provide:

```text
Load Balancing
TLS Termination
Security Filtering
Caching
Backend Protection
Traffic Routing
```

Example:

```text
Client
  ↓
Reverse Proxy
  ↓
Application Server
```

The backend server may not be directly exposed to the Internet.

---

## Forward Proxy vs Reverse Proxy

| Forward Proxy                  | Reverse Proxy                     |
| ------------------------------ | --------------------------------- |
| Represents clients             | Represents servers                |
| Client-side network            | Server-side infrastructure        |
| Controls outbound traffic      | Controls inbound traffic          |
| Hides or intermediates clients | Hides or protects backend servers |

Simple memory:

```text
Forward Proxy
Client → Proxy → Internet
```

```text
Reverse Proxy
Internet → Proxy → Server
```

---

# Complete Web Communication Flow

Suppose you open:

```text
https://example.com
```

The complete simplified process is:

## Step 1 - URL Entered

```text
https://example.com
```

The browser identifies:

```text
Protocol:
HTTPS

Domain:
example.com
```

---

## Step 2 - DNS Resolution

The browser needs the IP address.

```text
example.com
     ↓
DNS
     ↓
93.184.x.x
```

---

## Step 3 - Routing Decision

The operating system checks:

```text
Destination IP:
93.184.x.x
```

The destination is outside the local network.

The packet is sent to:

```text
Default Gateway
```

---

## Step 4 - ARP

The computer needs the gateway MAC address.

```text
Gateway IP
    ↓
ARP
    ↓
Gateway MAC
```

The Ethernet frame can now be created.

---

## Step 5 - TCP Three-Way Handshake

The client connects to:

```text
93.184.x.x:443
```

TCP handshake:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
```

TCP connection established.

---

## Step 6 - TLS Handshake

The client and server perform the TLS handshake.

Simplified:

```text
Client Hello
     ↓
Server Hello
Certificate
     ↓
Certificate Validation
     ↓
Key Agreement
     ↓
Session Keys
```

Encrypted communication is established.

---

## Step 7 - HTTP Request

The browser sends an HTTP request inside TLS.

Example:

```http
GET / HTTP/1.1
Host: example.com
```

Network view:

```text
HTTP Request
      ↓
TLS Encryption
      ↓
TCP
      ↓
IP
      ↓
Ethernet
```

---

## Step 8 - Server Processes Request

The server receives the request.

Possible flow:

```text
Reverse Proxy
      ↓
Web Server
      ↓
Application
      ↓
Database
```

The application creates a response.

---

## Step 9 - HTTP Response

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>
...
</html>
```

The response is encrypted by TLS.

---

## Step 10 - Browser Renders Website

The browser receives:

```text
HTML
CSS
JavaScript
Images
```

The browser processes the resources and displays the website.

Complete flow:

```text
URL
 ↓
DNS
 ↓
Routing
 ↓
ARP
 ↓
TCP Handshake
 ↓
TLS Handshake
 ↓
HTTP Request
 ↓
HTTP Response
 ↓
Browser Rendering
```

This connects almost every networking concept covered so far.

---

# Cybersecurity and SOC Relevance

HTTP, HTTPS, and TLS are extremely important for SOC analysts.

---

## 1. Web Proxy Logs

A web proxy log may contain:

```text
Timestamp
Source IP
User
HTTP Method
Domain
URL
Status Code
User-Agent
Bytes Sent
Bytes Received
Action
```

Example:

```text
src_ip=192.168.1.50

method=GET

domain=example.com

status=200

user_agent=Mozilla/5.0
```

SOC analysts use these logs to investigate web activity.

---

## 2. Suspicious HTTP Methods

Suppose a public web application receives many:

```text
DELETE
PUT
PATCH
```

requests.

If the application normally uses only:

```text
GET
POST
```

the activity may require investigation.

Example:

```text
Source IP
   ↓
100 DELETE Requests
   ↓
Multiple Resources
```

Questions include:

```text
Is the method expected?

Was authentication successful?

What status codes were returned?

Which URLs were targeted?

Did the requests modify data?
```

---

## 3. Repeated 401 Responses

Example:

```text
/login → 401

/login → 401

/login → 401

/login → 401
```

This may indicate:

```text
Incorrect Credentials
Application Problem
Automated Login Attempts
Brute Force Activity
```

The analyst should examine:

```text
Source IP
Username
Request Rate
User-Agent
Successful Login After Failures
```

---

## 4. Repeated 403 Responses

Example:

```text
/admin → 403

/config → 403

/backup → 403

/private → 403
```

This may indicate:

```text
Unauthorized Access Attempts
Directory Enumeration
Web Scanning
Misconfigured Application
```

The context is important.

---

## 5. Repeated 404 Responses

Example:

```text
/admin.php → 404

/wp-login.php → 404

/phpmyadmin → 404

/backup.zip → 404
```

A large number of requests to nonexistent resources may indicate:

```text
Web Enumeration
Automated Scanning
Vulnerability Scanning
```

Example pattern:

```text
One Source IP
      ↓
Many Different URLs
      ↓
Mostly 404 Responses
```

This can be useful for SOC detection.

---

## 6. HTTP User-Agent Analysis

Example normal-looking User-Agent:

```text
Mozilla/5.0
```

Example:

```text
python-requests/2.x
```

Another example:

```text
curl/x.x
```

Command-line or scripting User-Agents are not automatically malicious.

However, analysts may investigate unexpected User-Agents.

Example:

```text
Employee Workstation
       ↓
python-requests
       ↓
Unknown External Domain
```

Questions:

```text
Which process generated the traffic?

Is Python expected on the device?

What domain was contacted?

How much data was transferred?

Was the connection repeated?
```

---

## 7. Large Data Uploads

Suppose proxy logs show:

```text
Internal Host
     ↓
POST
     ↓
Unknown External Domain
     ↓
5 GB Uploaded
```

This may require investigation.

Possible causes:

```text
Cloud Backup
File Upload
Business Application
Data Exfiltration
Malware
```

Important fields:

```text
Source IP
User
Destination Domain
HTTP Method
Bytes Sent
Timestamp
Process Information
```

---

## 8. TLS Certificate Investigation

SOC analysts may inspect TLS certificates.

Useful certificate information includes:

```text
Subject
Issuer
Validity Period
Serial Number
Subject Alternative Names
Certificate Fingerprint
```

Possible suspicious indicators:

```text
Expired Certificate
Unexpected Issuer
Domain Mismatch
Newly Observed Certificate
Self-Signed Certificate
Unusual Validity Period
```

A self-signed certificate is not automatically malicious.

Internal systems and labs may legitimately use them.

Context is required.

---

## 9. SNI Analysis

TLS metadata may include a requested hostname through SNI.

Example:

```text
Source IP:
192.168.1.50

Destination IP:
45.x.x.x

SNI:
example.com
```

This can help analysts identify the hostname contacted even when HTTP application data is encrypted.

However, modern privacy technologies may reduce hostname visibility in some environments.

---

## 10. HTTP Beaconing

Malware may repeatedly contact a server.

Example:

```text
10:00 → example.com

10:05 → example.com

10:10 → example.com

10:15 → example.com

10:20 → example.com
```

Notice:

```text
Exactly Every 5 Minutes
```

This regular communication pattern may be called:

```text
Beaconing
```

Possible indicators:

```text
Regular Time Intervals
Same Destination
Similar Request Size
Similar Response Size
Repeated URI
```

SOC analysts may use network logs or a SIEM to identify periodic traffic.

---

## 11. Command and Control Traffic

A compromised system may communicate with attacker-controlled infrastructure.

Simplified:

```text
Compromised Host
       ↓
HTTPS
       ↓
Command and Control Server
```

HTTPS encryption can protect legitimate traffic.

Attackers may also use encrypted communication.

Therefore:

> HTTPS does not automatically mean the traffic is safe.

Analysts may investigate:

```text
Destination Reputation
Domain Age
Certificate Information
Traffic Frequency
Data Volume
Process Creating Connection
DNS History
User Context
```

---

## 12. HTTP Logs in Splunk

Web logs can be ingested into [Splunk](https://www.splunk.com/?utm_source=chatgpt.com).

Example event:

```text
src_ip=192.168.1.50

method=POST

uri=/login

status=401

user_agent=python-requests
```

A SOC analyst may search for repeated failed login attempts.

Example SPL idea:

```text
index=web status=401
```

Then group events by source IP.

```text
Source IP
   ↓
Count Failed Requests
```

Another investigation idea:

```text
One Source IP
      ↓
Many URLs
      ↓
Mostly 404
```

This may indicate web enumeration.

---

# Useful Commands

## curl

`curl` is a command-line tool used to communicate with network services.

Basic request:

```bash
curl http://example.com
```

HTTPS request:

```bash
curl https://example.com
```

Show response headers:

```bash
curl -I https://example.com
```

Verbose output:

```bash
curl -v https://example.com
```

Send a POST request:

```bash
curl -X POST https://example.com/login
```

Send form data:

```bash
curl -X POST \
-d "username=test&password=test" \
https://example.com/login
```

Send JSON:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"name":"Alex"}' \
https://example.com/api/users
```

Only test applications you own or have permission to test.

---

## OpenSSL

Inspect a TLS connection:

```bash
openssl s_client -connect example.com:443
```

Specify the server name:

```bash
openssl s_client \
-connect example.com:443 \
-servername example.com
```

The output may show:

```text
Certificate Chain
Server Certificate
TLS Version
Cipher
Verification Information
```

---

## Linux Network Commands

Show active TCP connections:

```bash
ss -tn
```

Show listening TCP ports:

```bash
ss -ltn
```

Show connections using port 443:

```bash
ss -tn | grep :443
```

DNS lookup:

```bash
dig example.com
```

Trace route:

```bash
traceroute example.com
```

---

## Windows Commands

DNS lookup:

```cmd
nslookup example.com
```

Trace route:

```cmd
tracert example.com
```

Show network connections:

```cmd
netstat -ano
```

Find HTTPS connections:

```cmd
netstat -ano | findstr :443
```

---

# Practical Labs

## Lab 1 - Inspect an HTTP Request

Run:

```bash
curl -v http://example.com
```

Observe:

```text
HTTP Method
Host Header
HTTP Version
Status Code
Response Headers
```

Try to identify:

```text
GET
Host
Content-Type
Content-Length
```

---

## Lab 2 - Compare HTTP and HTTPS

Run:

```bash
curl -v http://example.com
```

Then:

```bash
curl -v https://example.com
```

Observe the difference.

With HTTPS, look for TLS information.

Example:

```text
TLS Handshake
Certificate
Cipher
Encrypted Connection
```

---

## Lab 3 - Capture HTTP Traffic in Wireshark

Use a controlled lab environment.

Start Wireshark.

Apply:

```text
http
```

Generate HTTP traffic to a test service.

Observe:

```text
GET Request
HTTP Headers
Status Code
Response
```

Because HTTP is not encrypted, application data may be visible.

Do not send real passwords or sensitive information through an HTTP lab.

---

## Lab 4 - Capture HTTPS Traffic

Start Wireshark.

Apply:

```text
tls
```

Open an HTTPS website.

Observe:

```text
Client Hello
Server Hello
Certificate-Related Handshake Data
Application Data
```

Try to identify:

```text
TLS Version
Source IP
Destination IP
Source Port
Destination Port
SNI, if visible
```

Notice that the HTTP content is not normally readable from a basic packet capture because TLS encrypts the application data.

---

## Lab 5 - Inspect TLS Certificate

Run:

```bash
openssl s_client \
-connect example.com:443 \
-servername example.com
```

Look for:

```text
Certificate Chain
Subject
Issuer
TLS Version
Cipher
Verification Result
```

Try to answer:

```text
Who issued the certificate?

Which domain is the certificate for?

Is certificate verification successful?

Which TLS version was selected?
```

---

## Lab 6 - Inspect HTTP Status Codes

Run:

```bash
curl -I https://example.com
```

Observe the status code.

Try another URL.

Example:

```bash
curl -I https://example.com/not-found-page
```

Observe whether the server returns:

```text
404
```

Try to identify:

```text
2xx
3xx
4xx
5xx
```

responses during your web testing.

---

## Lab 7 - Python HTTP Request

Create:

```text
http_test.py
```

Add:

```python
import requests

url = "https://example.com"

response = requests.get(url)

print("Status Code:", response.status_code)
print("Headers:", response.headers)
print("Body:")
print(response.text)
```

Run:

```bash
python3 http_test.py
```

Observe:

```text
Status Code
Response Headers
Response Body
```

This connects Python networking with HTTP.

---

## Lab 8 - Analyze User-Agent

Create:

```python
import requests

url = "https://example.com"

headers = {
    "User-Agent": "Networking-Lab-Client"
}

response = requests.get(url, headers=headers)

print(response.status_code)
```

The request contains:

```text
User-Agent:
Networking-Lab-Client
```

This demonstrates that User-Agent values can be modified by clients.

Therefore:

> User-Agent data should be treated as useful context, not trusted identity.

---

## Lab 9 - Think Like a SOC Analyst

Suppose a proxy log shows:

```text
Time:
10:00:00

Source IP:
192.168.1.50

Method:
POST

Domain:
unknown-example.test

URI:
/api/check

Status:
200

Bytes Sent:
500

Bytes Received:
200
```

The same request occurs at:

```text
10:05:00

10:10:00

10:15:00

10:20:00
```

Questions:

```text
Is the timing regular?

Is the domain expected?

Which process created the connection?

Is the destination trusted?

Is the request size similar every time?

Could this be normal software update traffic?

Could this be beaconing?
```

Do not immediately declare the traffic malicious.

Investigate the complete context.

---

# What I Learned

After completing this section, I learned:

* HTTP stands for Hypertext Transfer Protocol.
* HTTP is an application-layer protocol.
* HTTP uses a client-server communication model.
* Clients send HTTP requests.
* Servers send HTTP responses.
* HTTP commonly uses TCP port 80.
* An HTTP request contains a request line, headers, and optionally a body.
* HTTP methods describe actions a client wants to perform.
* GET retrieves data.
* POST sends or creates data.
* PUT commonly replaces or updates a complete resource.
* PATCH partially updates a resource.
* DELETE requests resource deletion.
* HEAD retrieves response headers without the normal response body.
* OPTIONS can identify supported HTTP methods.
* HTTP status codes describe request results.
* 2xx status codes indicate success.
* 3xx status codes indicate redirection.
* 4xx status codes indicate client-side request errors.
* 5xx status codes indicate server-side errors.
* 200 means OK.
* 301 indicates a permanent redirect.
* 401 is related to authentication requirements or failure.
* 403 means access is forbidden.
* 404 means the resource was not found.
* 429 indicates too many requests.
* 500 indicates an internal server error.
* HTTP headers provide additional request and response information.
* The Host header identifies the requested hostname.
* The User-Agent header provides client information but can be modified.
* Content-Type identifies the format of the message body.
* Cookies store small pieces of browser data.
* Sessions help servers maintain user state.
* HTTP is stateless by itself.
* HTTPS means HTTP protected using TLS.
* HTTPS commonly uses TCP port 443.
* TLS stands for Transport Layer Security.
* TLS provides confidentiality, integrity, and authentication.
* Encryption converts plaintext into ciphertext.
* Symmetric encryption uses a shared secret key.
* Asymmetric cryptography uses public and private keys.
* TLS combines multiple cryptographic mechanisms.
* Digital certificates help verify server identities.
* Certificate Authorities issue and sign certificates.
* Certificate chains may include root, intermediate, and server certificates.
* Self-signed certificates are not automatically trusted by public browser trust stores.
* The TLS handshake establishes secure communication.
* The Client Hello contains client-supported TLS information.
* The Server Hello contains selected TLS parameters.
* Clients validate server certificates.
* TLS key agreement helps establish shared session keys.
* HTTP application data is encrypted after secure TLS communication is established.
* SNI can identify the requested server hostname in some TLS traffic.
* A forward proxy represents clients.
* A reverse proxy represents servers.
* Proxy logs are valuable for SOC investigations.
* Repeated 401 responses may indicate failed authentication attempts.
* Repeated 403 and 404 responses may indicate web enumeration or scanning.
* Large unexpected uploads may require data exfiltration investigation.
* TLS certificate metadata can help investigate network destinations.
* HTTPS traffic is encrypted but is not automatically safe.
* Regular repeated communication may indicate beaconing.
* HTTP and proxy logs can be analyzed in Splunk and other SIEM platforms.
* `curl` can inspect HTTP requests and responses.
* `openssl s_client` can inspect TLS connections and certificates.
* Wireshark can help analyze HTTP and TLS network traffic.
