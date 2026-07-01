# Load Balancing

Load Balancers are servers that forward incoming traffic to multiple downstream servers (e.g., EC2 instances).

## Benefits of Load Balancers

- Spread traffic across multiple downstream instances.
- Expose a single point of access (DNS) to your application.
- Seamlessly handle failures of downstream instances.
- Perform regular health checks on instances.
- Provide SSL termination (HTTPS) for websites.
- Support session stickiness using cookies.
- Enable high availability across multiple Availability Zones.
- Separate public traffic from private traffic.

---

# Health Checks

Health Checks are essential for Load Balancers.

They allow the Load Balancer to determine whether an instance is healthy and able to handle incoming requests.

## How Health Checks Work

- Performed against a specific **port** and **endpoint** (commonly `/health`).
- The Load Balancer periodically sends requests to the configured endpoint.
- If the instance responds with **HTTP 200 (OK)**, it is considered **healthy**.
- If the response is anything other than **200 (OK)**, the instance is marked as **unhealthy** and traffic is no longer routed to it.

---

# Application Load Balancer (ALB)

Application Load Balancer (ALB) operates at **Layer 7 (HTTP/HTTPS)** of the OSI model.

## Features

- Load balances traffic across multiple HTTP applications running on different machines (Target Groups).
- Load balances traffic to multiple applications running on the same machine (e.g., containers).
- Supports **HTTP/2** and **WebSockets**.
- Supports redirects (e.g., HTTP → HTTPS).
