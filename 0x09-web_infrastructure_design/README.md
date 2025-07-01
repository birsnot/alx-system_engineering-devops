![Simple web stack](simple-web-stack.png)

## Explanations

### 1. **What is a server?**

A server is a physical or virtual computer that provides services or resources to other computers (clients). In this case, it's hosting the web app.

### 2. **What is the role of the domain name?**

The domain name (foobar.com) maps a human-readable name to the IP address of the server (8.8.8.8), so users don’t need to remember IPs.

### 3. **What type of DNS record is 'www' in [www.foobar.com](http://www.foobar.com)?**

The `www` is a subdomain, and the DNS record type used is typically an **A record** (Address Record) that maps the name to the server’s IP address (8.8.8.8).

### 4. **What is the role of the web server (Nginx)?**

The web server handles HTTP requests, serves static files (like HTML/CSS), and forwards dynamic requests to the application server.

### 5. **What is the role of the application server?**

The application server (e.g. Gunicorn for Python) runs the backend logic/code (e.g. Flask, Django, Node.js app) and handles dynamic content generation.

### 6. **What is the role of the database (MySQL)?**

The database stores and manages application data, such as user accounts, posts, transactions, etc., and responds to queries from the application server.

### 7. **How does the server communicate with the user’s computer?**

Through the **Internet**, using **TCP/IP protocols** like HTTP/HTTPS over port 80/443. The web server uses HTTP(S) to serve the user’s browser.

---

## Issues with this Infrastructure

* **SPOF (Single Point of Failure):** Only one server. If it fails, everything goes down.
* **Downtime during maintenance:** Restarting the web server to update code will bring down the entire site.
* **Not scalable:** A single server can only handle a limited number of requests. Under heavy traffic, it will crash or become slow.