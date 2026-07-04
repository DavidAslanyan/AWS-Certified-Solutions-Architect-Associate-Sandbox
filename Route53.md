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
