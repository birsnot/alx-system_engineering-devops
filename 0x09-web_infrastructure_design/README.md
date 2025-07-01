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


---
![Distributed web infrastructure](distributed-web-infrastructure.png)

## Explanations

### **Why Each New Element Was Added**

* **Load Balancer (HAproxy)**: Distributes incoming traffic between web servers to improve availability and scalability.
* **Two Web Servers**: To support more traffic and add redundancy—if one fails, the other still serves users.
* **Application Server**: Hosts the backend logic and processes dynamic content.
* **MySQL Database**: Stores and manages persistent data (users, posts, etc.).

---

### **Load Balancer Setup**

* **Distribution Algorithm**: Typically `Round Robin`.

  * Works by sending each new request to the next server in line, looping back to the first after the last.
  * Example: Request 1 → Server A, Request 2 → Server B, Request 3 → Server A, and so on.

---

### **Active-Active vs Active-Passive**

* **Your setup is Active-Active**:

  * Both web servers are running and serving traffic simultaneously.
* **Active-Active**:

  * All servers handle traffic.
  * Better for load sharing and performance.
* **Active-Passive**:

  * One server is idle and only takes over if the active one fails.
  * Used more for high availability/failover, not performance.

---

### **Database Cluster: Primary-Replica (Master-Slave)**

(This setup has only one DB node, but here's how the concept works.)

* **Primary Node**: Handles **writes** (INSERT, UPDATE, DELETE).
* **Replica Node**: Handles **reads** (SELECT), syncing data from the Primary.
* Used to **offload read operations** and for **fault tolerance**.

---

### **Difference Between Primary and Replica**

* **Primary**:

  * Handles read and write operations.
  * Source of truth.
* **Replica**:

  * Only handles read queries.
  * Cannot modify data; syncs from the Primary.

---

## Issues with This Infrastructure

* **SPOF (Single Point of Failure)**:

  * The **load balancer** is a SPOF. If it fails, the site becomes unreachable.
  * The **database** is also a SPOF—no replication yet.
* **Security Issues**:

  * No firewall to control incoming/outgoing access.
  * No HTTPS → data can be intercepted.
* **No Monitoring**:

  * No system in place to detect failures, performance degradation, etc.