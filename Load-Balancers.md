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

# Network Load Balancer (NLB)

Network Load Balancer (NLB) operates at **Layer 4 (Transport Layer)** of the OSI model.

## Features

- Forwards **TCP** and **UDP** traffic to your instances.
- Handles millions of requests per second.
- Provides ultra-low latency.
- Has one **static IP address per Availability Zone (AZ)**.
- Supports assigning **Elastic IPs** (useful for IP whitelisting).
- Best suited for extreme performance and TCP/UDP workloads.

---

# Network Load Balancer – Target Groups

NLB can route traffic to the following target types:

- EC2 Instances
- IP Addresses (must be private IPs)
- Application Load Balancers (ALBs)

## Health Checks

NLB supports health checks using:

- TCP
- HTTP
- HTTPS

---

# Gateway Load Balancer (GWLB)

Gateway Load Balancer (GWLB) is designed to deploy, scale, and manage fleets of third-party virtual network appliances in AWS.

## Common Use Cases

- Firewalls
- Intrusion Detection Systems (IDS)
- Intrusion Prevention Systems (IPS)
- Deep Packet Inspection (DPI)
- Payload manipulation
- Other network security appliances

## Characteristics

- Operates at **Layer 3 (Network Layer)** of the OSI model (IP Packets).
- Combines two key functions:
  - **Transparent Network Gateway** – provides a single entry and exit point for all traffic.
  - **Load Balancer** – distributes traffic across multiple virtual appliances.
- Uses the **GENEVE** protocol on **port 6081**.
