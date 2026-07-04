# Amazon Route 53

**Amazon Route 53** is AWS's fully managed **Domain Name System (DNS)** service.

It is designed to be:

- Highly available
- Scalable
- Fully managed
- An **Authoritative DNS** service

---

# What is Authoritative DNS?

An **Authoritative DNS** server stores the official DNS records for a domain.

With Route 53, **you** control and update the DNS records for your domains.

For example, you can configure where requests for:

- `example.com`
- `api.example.com`
- `www.example.com`

should be routed.

---

# Key Features

## Domain Registration

Route 53 can act as a **Domain Registrar**, allowing you to:

- Register new domain names
- Transfer existing domains
- Manage domain settings

## DNS Management

Create and manage DNS records for your domains.

## Health Checks

Route 53 can continuously monitor the health of AWS resources and external endpoints.

It can automatically stop routing traffic to unhealthy resources.

## High Availability

Route 53 is the **only AWS service that provides a 100% Availability SLA**.

---

# Why Is It Called Route 53?

The name **Route 53** comes from **port 53**, the standard network port used by the DNS protocol.

---

# Route 53 Record Types

DNS records define how traffic is routed for a domain.

## A Record

Maps a hostname to an **IPv4 address**.

### Example

```text
example.com  →  192.168.1.10
```

---

## AAAA Record

Maps a hostname to an **IPv6 address**.

### Example

```text
example.com  →  2001:db8::1
```

---

## CNAME Record

A **Canonical Name (CNAME)** record maps one hostname to another hostname.

Instead of pointing directly to an IP address, it points to another domain name.

### Example

```text
www.example.com
        ↓
example.com
        ↓
192.168.1.10
```

### Requirements

The target hostname must already have:

- An A record, or
- An AAAA record

### Limitation

A CNAME **cannot** be created for the **Zone Apex** (root domain).

For example:

❌ Not allowed

```text
example.com → anotherdomain.com
```

✅ Allowed

```text
www.example.com → example.com
```

---

## NS Record (Name Server)

NS records specify the **authoritative name servers** for a hosted zone.

They determine which DNS servers are responsible for answering queries for your domain.

---

# Summary of Record Types

| Record Type | Purpose |
|-------------|---------|
| **A** | Maps a hostname to an IPv4 address |
| **AAAA** | Maps a hostname to an IPv6 address |
| **CNAME** | Maps one hostname to another hostname |
| **NS** | Specifies the authoritative name servers for a domain |

---

# How Route 53 Routes Traffic

When a user enters a domain name (e.g., `example.com`) into their browser:

1. Route 53 receives the DNS query.
2. It looks up the appropriate DNS record.
3. It returns the correct IP address (or another hostname).
4. The user's browser connects to the destination server.

Route 53 can also use **routing policies** and **health checks** to intelligently direct traffic to healthy resources across AWS or external infrastructure.

md id="t8n4vz"
# Amazon Route 53 Routing Policies

Route 53 **Routing Policies** determine **how DNS queries are answered** and which resource receives incoming traffic.

They allow you to optimize for:

- Availability
- Performance
- Geographic location
- Load balancing
- Disaster recovery

---

# Weighted Routing Policy

The **Weighted Routing Policy** distributes traffic across multiple resources based on assigned weights.

Each DNS record is assigned a relative weight.

## Traffic Distribution

Traffic percentage is calculated as:

```text
Traffic % = Record Weight / Sum of All Record Weights
```

For example:

| Resource | Weight | Approximate Traffic |
|----------|-------:|--------------------:|
| Server A | 70 | 70% |
| Server B | 20 | 20% |
| Server C | 10 | 10% |

> **Note:** The weights do **not** need to add up to 100. Route 53 automatically calculates the percentages.

## Requirements

All weighted records must have the:

- Same record name
- Same record type

## Features

- Can be combined with **Health Checks**.
- Setting a record's weight to **0** stops sending traffic to that resource (unless all records have a weight of 0).

## Common Use Cases

- Load balancing across AWS Regions
- Blue/Green deployments
- Canary releases
- Testing new application versions

---

# Latency-Based Routing Policy

Latency-Based Routing sends users to the AWS Region that provides the **lowest network latency**, not necessarily the geographically closest one.

For example:

A user in Germany may be routed to **us-east-1** instead of a European Region if it offers lower network latency.

## Features

- Optimizes user experience.
- Based on AWS's measured network latency.
- Can be combined with **Health Checks**.

## Common Use Cases

- Global applications
- Gaming
- Streaming services
- APIs requiring low response times

---

# Route 53 Health Checks

Health Checks allow Route 53 to determine whether a resource is healthy before routing traffic to it.

If a resource becomes unhealthy, Route 53 can automatically stop directing traffic to it.

## Types of Health Checks

### 1. Endpoint Health Checks

Monitor public endpoints such as:

- Web applications
- Web servers
- AWS resources
- External resources

> **HTTP/HTTPS health checks work only for publicly accessible resources.**

---

### 2. Calculated Health Checks

Monitor the results of multiple existing health checks.

Useful for creating complex health rules.

---

### 3. CloudWatch Alarm Health Checks

Monitor the state of **Amazon CloudWatch Alarms**.

This is useful for monitoring private AWS resources that cannot be checked over HTTP.

Examples include:

- Amazon RDS
- Amazon DynamoDB throttling
- Custom CloudWatch metrics

## Integration

Health Checks are integrated with **Amazon CloudWatch** metrics.

---

# Geolocation Routing Policy

Geolocation Routing directs users based on **where the user is physically located**.

It does **not** consider network latency.

You can define routing rules based on:

- Continent
- Country
- US State

If multiple rules apply, Route 53 selects the **most specific** location.

## Default Record

You should always configure a **Default** record to handle users whose locations do not match any defined rule.

## Common Use Cases

- Localized websites
- Country-specific content
- Legal or licensing restrictions
- Regional load balancing

## Features

- Can be combined with **Health Checks**.

---

# Geoproximity Routing Policy

Geoproximity Routing directs traffic based on the geographic locations of both:

- Users
- Resources

Unlike Geolocation Routing, you can influence how much traffic each resource receives by applying a **bias**.

## Bias Values

| Bias | Effect |
|------:|--------|
| 1 to 99 | Expands the region, sending more traffic to the resource |
| -1 to -99 | Shrinks the region, sending less traffic to the resource |

## Supported Resource Types

- AWS resources (using AWS Regions)
- Non-AWS resources (using latitude and longitude)

## Requirement

Geoproximity Routing requires **Route 53 Traffic Flow**.

## Common Use Cases

- Fine-grained geographic traffic control
- Global applications
- Regional traffic optimization

---

# IP-Based Routing Policy

IP-Based Routing directs traffic based on the **client's IP address**.

Instead of determining the user's physical location, Route 53 compares the client's IP against a list of configured **CIDR blocks**.

Each CIDR block is mapped to a specific endpoint.

## Example

| Client CIDR | Destination |
|-------------|-------------|
| 203.0.113.0/24 | Endpoint A |
| 200.5.4.0/24 | Endpoint B |

If a user's IP belongs to one of these CIDR ranges, Route 53 routes them to the corresponding endpoint.

## Common Use Cases

- Direct users from a specific ISP to a preferred endpoint
- Reduce networking costs
- Improve application performance
- Enterprise network routing

---

# Summary of Route 53 Routing Policies

| Routing Policy | Routes Based On | Common Use Cases |
|----------------|-----------------|------------------|
| **Simple** | Single resource | Basic DNS routing |
| **Weighted** | Assigned weights | Load balancing, Blue/Green deployments, A/B testing |
| **Latency-Based** | Lowest network latency | Performance optimization |
| **Failover** | Health check status | Disaster recovery and high availability |
| **Geolocation** | User's geographic location | Localized content, regional restrictions |
| **Geoproximity** | User location + resource location + bias | Fine-grained geographic traffic control |
| **IP-Based** | Client IP (CIDR blocks) | ISP-specific routing, enterprise networking |
| **Multi-Value Answer** | Multiple healthy endpoints | Simple load balancing with health checks |

