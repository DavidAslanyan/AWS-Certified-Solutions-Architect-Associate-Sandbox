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


# Sticky Sessions (Session Affinity)

Sticky Sessions (also known as **Session Affinity**) ensure that the same client is consistently routed to the same backend instance behind a Load Balancer.

## Features

- The same client is always redirected to the same EC2 instance.
- Supported by:
  - Classic Load Balancer (CLB)
  - Application Load Balancer (ALB)
  - Network Load Balancer (NLB)
- For **CLB** and **ALB**, stickiness is implemented using **cookies**.
- The cookie expiration time is configurable.

## Use Case

- Ensures users do not lose their session data when making multiple requests.

## Considerations

- Enabling stickiness can lead to an uneven distribution of traffic across backend EC2 instances.

---

# SSL/TLS Basics

An **SSL/TLS certificate** encrypts traffic between clients and your Load Balancer, providing **encryption in transit**.

## SSL vs TLS

- **SSL (Secure Sockets Layer)** is the original protocol for encrypting network connections.
- **TLS (Transport Layer Security)** is the newer, more secure version of SSL.
- Although TLS is used today, the term **SSL certificate** is still commonly used.

## Certificate Authorities (CAs)

Public SSL/TLS certificates are issued by trusted **Certificate Authorities (CAs)**.

Examples include:

- Comodo
- Symantec
- GoDaddy
- GlobalSign
- DigiCert
- Let's Encrypt

## Certificate Expiration

- SSL/TLS certificates have an expiration date.
- Certificates must be renewed before they expire to maintain secure connections.

# Auto Scaling Groups (ASG)

## What is an Auto Scaling Group?

In real-world applications, traffic and workload demands constantly change.

Because cloud resources can be provisioned and terminated quickly, AWS provides **Auto Scaling Groups (ASGs)** to automatically adjust the number of EC2 instances based on demand.

## Goals of an Auto Scaling Group

- **Scale Out** (add EC2 instances) when load increases.
- **Scale In** (remove EC2 instances) when load decreases.
- Maintain a minimum number of running EC2 instances.
- Enforce a maximum number of running EC2 instances.
- Automatically register new instances with a Load Balancer.
- Automatically replace unhealthy or terminated instances.

## Pricing

- Auto Scaling Groups themselves are **free**.
- You only pay for the underlying EC2 instances and other AWS resources used.

---

# Auto Scaling Group Scaling Policies

Scaling policies determine when and how an ASG adds or removes EC2 instances.

## Dynamic Scaling

Automatically adjusts capacity based on metrics and demand.

### Target Tracking Scaling

- Simple to configure.
- AWS automatically scales the ASG to maintain a target metric value.

**Example:**

```text
Target Average CPU Utilization = 40%
```

If CPU usage rises above the target, AWS adds instances.

If CPU usage falls below the target, AWS removes instances.

---

### Simple / Step Scaling

Uses CloudWatch alarms to trigger scaling actions.

**Examples:**

```text
CPU > 70%  → Add 2 instances
```

```text
CPU < 30%  → Remove 1 instance
```

Scaling actions occur when the associated CloudWatch alarm is triggered.

---

## Scheduled Scaling

Used when traffic patterns are predictable.

You define scaling actions that occur at specific times.

**Example:**

```text
Every Friday at 5:00 PM
Increase minimum capacity to 10 instances
```

Useful for:

- Business hours traffic spikes
- Weekly events
- Marketing campaigns
- Seasonal demand patterns
