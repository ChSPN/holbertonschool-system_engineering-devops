
# README - Web Infrastructure for www.foobar.com

## Project Description

This project describes a robust and scalable web infrastructure for a website accessible via the domain `www.foobar.com`. The infrastructure includes separate servers for different functions (web server, application server, database), a load balancer cluster to distribute traffic, and additional components to enhance security (firewalls, SSL) and monitoring (data collectors).

---

## Architecture Overview

### Infrastructure Components

1. **2 Load Balancers (HAProxy) Configured in a Cluster:**
   - One **Active** and one **Passive** load balancer for redundancy and high availability.
   
2. **Dedicated Servers for Each Component:**
   - **Web Servers (Nginx)**: Handle static content and forward dynamic requests to the application servers.
   - **Application Servers**: Process dynamic requests, execute business logic, and interact with the database.
   - **Database Servers (MySQL Primary-Replica)**: Primary node handles write operations, and Replica nodes handle read operations.

3. **3 Firewalls:**
   - Protect each layer of the infrastructure:
     - **Firewall 1**: Secures the load balancers by filtering incoming traffic.
     - **Firewall 2**: Controls traffic between the load balancers and application servers.
     - **Firewall 3**: Protects the database, allowing access only from the application servers.

4. **SSL Certificate:**
   - Ensures all traffic between the user's browser and the load balancer is encrypted using HTTPS.

5. **3 Monitoring Clients:**
   - Deployed on the load balancers, application servers, and database servers to collect performance data and logs for analysis.

---

## Detailed Component Breakdown

### 1. Load Balancer Cluster (HAProxy)
- **Purpose**: Distributes incoming traffic across multiple web servers.
- **Configuration**: Active-Passive setup with two load balancers for high availability. If the active load balancer fails, the passive one automatically takes over.
- **Algorithm**: Round Robin or Least Connections to ensure efficient traffic distribution.

### 2. Dedicated Web Servers (Nginx)
- **Purpose**: Serve static files (HTML, CSS, JavaScript) and forward dynamic requests to the application servers.
- **Why Dedicated**: Offloading static content reduces the load on the application servers, improving performance.

### 3. Dedicated Application Servers
- **Purpose**: Execute server-side code, handle user requests, and interact with the database.
- **Why Dedicated**: Allows independent scaling and better resource allocation for dynamic content processing.

### 4. Dedicated Database Servers (MySQL Primary-Replica)
- **Primary Node**: Handles all write operations.
- **Replica Node(s)**: Handle read operations, improving performance and distributing load.
- **Why Dedicated**: Separating the database from the application server allows independent scaling and better resource management.

---

## Security Measures

### Firewalls
- **Purpose**: Provide layers of protection by restricting access to the various components of the infrastructure.
- **Why Important**: They prevent unauthorized access and protect sensitive data by only allowing legitimate traffic between components.

### SSL Certificate
- **Purpose**: Encrypt all traffic between the user and the server to prevent data interception.
- **Why Important**: Ensures secure communication, protects user data, and verifies the website’s identity.

---

## Monitoring

### Monitoring Clients (e.g., Sumo Logic, Prometheus)
- **Purpose**: Collect data on performance metrics (CPU, memory, network) and logs (access logs, error logs) to ensure the infrastructure runs smoothly.
- **Why Important**: Provides visibility into the health of the infrastructure, allowing administrators to detect and fix issues early.
- **QPS Monitoring**: The monitoring client can track Queries Per Second (QPS) on the web servers to monitor the traffic load.

---

## Issues and Considerations

### SSL Termination at Load Balancer (Potential Issue)
- **Problem**: If SSL is terminated at the load balancer, traffic between the load balancer and the application servers is unencrypted, which could expose sensitive data.
- **Solution**: Consider end-to-end encryption by configuring SSL certificates on both the load balancer and the application servers.

### Single MySQL Primary Node for Writes (Scalability Issue)
- **Problem**: Only one MySQL Primary node handles all write operations. If it fails, the system cannot handle writes until a Replica is promoted.
- **Solution**: Implement automatic failover or explore multi-master replication to distribute write operations across multiple nodes.

### Servers with All the Same Components (Problematic for Scaling)
- **Problem**: If each server runs web, application, and database services, resource contention can occur, and scaling individual components becomes difficult.
- **Solution**: Splitting components into dedicated servers, as outlined in this infrastructure, solves this by allowing independent scaling and resource optimization.

---

## Conclusion

This infrastructure design ensures high availability, scalability, security, and monitoring capabilities for a robust web application. By separating components into dedicated servers and using load balancer clustering and firewalls, this architecture addresses many challenges of traditional single-server setups, while enhancing performance and fault tolerance.