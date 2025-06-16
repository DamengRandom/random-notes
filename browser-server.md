After user typed an URL, what happens next?

Step 1: URL parsing: browser parses the URL into components, eg:

https://www.example.com:443/path/page?query=123#section

- Protocol: https
- Host: www.example.com
- Port: 443
- Path: /path/page
- Query: query=123
- Section: section

Step 2: Check browser cache

- Browser will check if the resource is cached
- If it is, browser will use the cached resource
- If it is not, browser will send a request to the server

Step 3: DNS resolution

- Browser will resolve the domain name to an IP address
- Browser will send a request to the server
- Server will send a response to the browser

Step 4: TCP connection establishment

- Browser opens a TCP connection to server IP address at port 443 (HTTPS)
- Use 3 way handshake to establish the connection (SYN, SYN-ACK, ACK)
(Until now, server and client browser have established a connection ✅)

Step 5: TLS (Transport Layer Security) / SSL (Secure Sockets Layer) handshake

- Browser sends supported ciphers, random unmber.
- Server sends SSL certificate + public key
- Browser verifies server cerificate (via CA root store)
- Browser generates a pre-master secret key -> encrypt it with server's public key -> send to server
- Both sides derive session keys (Symmetric encryption)
- Security encrypted connection is established ✅

Step 6: HTTP request sent

- browser sends HTTP GET request to grab the web page contents

Step 7: Server processes the request

- Reads cookie, headers, queries
- authenticate user via session / token / cookies
- generate the HTML contents response

Step 8: Browser response retuned

- Server sends the response back to the browser with HTML contents

Step 9: Browser renders the page

- Browser parses the HTML to build the DOM tree, linked with CSS & JavaScript (HTML -> CSS -> JavaScript)

Step 10: JavaScript starts the event loop

- Browser starts to listen user behaviours, eg: click, fetch more data, run web APIs, eg: fetch(), setTimeout() and event listeners. (In a word, browser starts handling user events)
