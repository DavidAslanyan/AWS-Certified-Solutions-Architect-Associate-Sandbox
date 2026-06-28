
# Amazon EC2

**Amazon Elastic Compute Cloud (EC2)** is an **Infrastructure as a Service (IaaS)** offering that provides scalable virtual servers in the cloud.

## EC2 Consists Of

- Renting virtual machines (**EC2 Instances**)
- Storing data on virtual drives (**EBS - Elastic Block Store**)
- Distributing traffic across multiple machines (**ELB - Elastic Load Balancer**)
- Automatically scaling services (**ASG - Auto Scaling Group**)

---

# EC2 Sizing & Configuration Options

When launching an EC2 instance, you can configure:

## Operating System (OS)

- Linux
- Windows
- macOS

## Compute Resources

- Number of CPUs (vCPUs)
- Processing power

## Memory

- Amount of RAM (Random Access Memory)

## Storage

### Network-Attached Storage

- **EBS (Elastic Block Store)**
- **EFS (Elastic File System)**

### Hardware Storage

- **EC2 Instance Store** (physically attached storage)

## Networking

- Network card performance and speed
- Public IP address assignment

## Security

- Firewall rules using **Security Groups**

## Bootstrap Configuration

- **EC2 User Data** scripts that run when the instance starts

---

# EC2 User Data

**EC2 User Data** allows you to automate tasks that run when an instance is launched.

Common use cases include:

- Installing operating system updates
- Installing software and dependencies
- Downloading files from the internet
- Configuring applications and services
- Running custom startup scripts
- Any other initialization task

## Important Notes

- User Data scripts are typically executed during the first boot of the instance.
- The script runs with **root (administrator) privileges**, giving it full access to configure the system.
- User Data helps automate server setup and reduces manual configuration effort.

---

# Example Use Case

When launching a new web server, an EC2 User Data script can automatically:

1. Install system updates
2. Install a web server (e.g., Apache or Nginx)
3. Download application files
4. Start required services

This allows the server to be ready for use immediately after launch.

# AWS EC2 Instance Types

## EC2 Instance Types

AWS provides different EC2 instance types optimized for specific workloads.

Reference:
- https://aws.amazon.com/ec2/instance-types/

---

## Instance Type Naming Convention

Example:

```text
m5.2xlarge
```

### Breakdown

| Component | Meaning |
|------------|---------|
| `m` | Instance family/class |
| `5` | Generation (newer generations provide improved performance and features) |
| `2xlarge` | Instance size within the family |

---

## Main EC2 Instance Categories

### 1. General Purpose Instances

General-purpose instances provide a balanced mix of:

- Compute
- Memory
- Networking

#### Use Cases

- Web servers
- Application servers
- Code repositories
- Small and medium databases
- Development and testing environments

#### Example Families

- M series (e.g., `m5`, `m6i`, `m7i`)

---

### 2. Compute Optimized Instances (C Series)

Designed for compute-intensive workloads requiring high-performance processors.

#### Use Cases

- Batch processing workloads
- Media transcoding
- High-performance web servers
- High Performance Computing (HPC)
- Scientific modeling
- Machine learning
- Dedicated gaming servers

#### Example Families

- C series (e.g., `c5`, `c6i`, `c7i`)

**Exam Tip:**  
Choose Compute Optimized instances when CPU performance is the primary requirement.

---

### 3. Memory Optimized Instances (R Series)

Designed for workloads that process large datasets in memory.

#### Benefits

- High memory-to-vCPU ratio
- Faster access to data stored in RAM

#### Use Cases

- High-performance relational databases
- High-performance NoSQL databases
- Distributed web-scale cache stores
- In-memory databases
- Business Intelligence (BI) applications
- Real-time big data processing

#### Example Families

- R series (e.g., `r5`, `r6i`, `r7i`)

**Exam Tip:**  
If the question mentions large datasets, caching, or in-memory processing, think **Memory Optimized**.

---

### 4. Storage Optimized Instances

Designed for storage-intensive workloads requiring high sequential read/write performance.

#### Benefits

- High IOPS (Input/Output Operations Per Second)
- Low latency storage access
- Optimized local storage performance

#### Use Cases

- High-frequency Online Transaction Processing (OLTP)
- Relational databases
- NoSQL databases
- Caching for in-memory databases (e.g., Redis)
- Data warehousing
- Distributed file systems

#### Example Families

- I series
- D series

**Exam Tip:**  
If the workload requires extremely fast disk access or large local storage, choose **Storage Optimized**.

---

## Quick Memory Trick for the Exam

| Family | Meaning | Best For |
|----------|---------|----------|
| **M** | General Purpose | Balanced workloads |
| **C** | Compute Optimized | CPU-intensive applications |
| **R** | Memory Optimized | RAM-intensive applications |
| **I / D** | Storage Optimized | High-performance storage workloads |

### Easy Way to Remember

- **M = Mixed resources**
- **C = CPU**
- **R = RAM**
- **I/D = Disk**


# AWS Security Groups

## Security Groups Overview

Security Groups are the fundamental building blocks of network security in AWS.

They act as a **virtual firewall** for EC2 instances and control what network traffic is allowed to enter or leave an instance.

---

## Key Characteristics

- Security Groups contain **only allow rules**
  - There are no explicit deny rules.
- Rules can reference:
  - IP addresses (IPv4 or IPv6)
  - Other Security Groups
- Security Groups are stateful:
  - If inbound traffic is allowed, the response is automatically allowed back out.
  - If outbound traffic is allowed, the response is automatically allowed back in.

---

## What Security Groups Control

### Inbound Rules

Control traffic coming **to** the EC2 instance.

Examples:

- Allow SSH (Port 22) from your IP
- Allow HTTP (Port 80) from anywhere
- Allow HTTPS (Port 443) from anywhere

### Outbound Rules

Control traffic leaving **from** the EC2 instance.

Examples:

- Allow access to the internet
- Allow communication with databases
- Allow access to AWS services

---

## Security Groups Act as a Firewall

Security Groups regulate:

- Access to specific ports
- Authorized IPv4 and IPv6 ranges
- Inbound traffic (external → EC2)
- Outbound traffic (EC2 → external)

```text
Internet
    ↓
Security Group
    ↓
EC2 Instance
```

Traffic must pass through the Security Group before reaching the EC2 instance.

---

## Important Facts for the Exam

### Security Groups Can Be Shared

- A Security Group can be attached to multiple EC2 instances.
- Multiple Security Groups can be attached to a single EC2 instance.

### Region and VPC Scoped

Security Groups are tied to:

- A specific AWS Region
- A specific VPC

They cannot be shared across Regions or VPCs.

### Operate Outside the EC2 Instance

Security Groups exist outside the EC2 operating system.

If traffic is blocked:

- The EC2 instance never sees it.
- No logs are generated by the application for blocked traffic.

---

## Best Practices

### Separate SSH Access

Create a dedicated Security Group for SSH access.

Example:

| Port | Protocol | Source |
|--------|----------|----------|
| 22 | TCP | Your public IP |

Benefits:

- Easier management
- Better security
- Reduced attack surface

---

## Troubleshooting Guide

### Application Times Out

Usually indicates a **Security Group issue**.

Possible causes:

- Required port not opened
- Incorrect source IP range
- Missing inbound rule
- Missing outbound rule

```text
Request → No Response → Timeout
```

Think: **Security Group or Networking Problem**

---

### Connection Refused

Usually indicates an **application issue**.

Possible causes:

- Application is not running
- Application is listening on the wrong port
- Service crashed
- Web server not started

```text
Request → Server Responds "No" → Connection Refused
```

Think: **Application Problem**

---

## Default Behavior

### Inbound Traffic

❌ Blocked by default

No one can connect to your EC2 instance until you explicitly create inbound rules.

### Outbound Traffic

✅ Allowed by default

EC2 instances can initiate outbound connections unless outbound rules are restricted.

---

## Common Ports to Memorize for the Exam

| Port | Service |
|--------|---------|
| 22 | SSH |
| 21 | FTP |
| 22 | SFTP |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP (Windows Remote Desktop) |

---

## Exam Tips

### Timeout vs Connection Refused

| Error | Most Likely Cause |
|---------|------------------|
| Timeout | Security Group / Network issue |
| Connection Refused | Application not running |

### Remember

- Security Groups = Virtual Firewall for EC2
- Only Allow Rules
- Stateful
- Inbound Blocked by Default
- Outbound Allowed by Default
- Can Reference Other Security Groups
- Region + VPC Scoped

### Easy Memory Trick

**SG = Security Group = Shield Guard**

Think of Security Groups as guards standing in front of your EC2 instance, allowing only approved traffic to pass through.

# Common AWS Ports to Know

These ports are frequently referenced in AWS exams and real-world deployments.

| Port | Protocol / Service | Purpose |
|--------|------------------|---------|
| **22** | SSH (Secure Shell) | Log in to Linux EC2 instances |
| **21** | FTP (File Transfer Protocol) | Transfer files to/from a server |
| **22** | SFTP (Secure File Transfer Protocol) | Secure file transfer over SSH |
| **80** | HTTP | Access unsecured websites |
| **443** | HTTPS | Access secured websites using SSL/TLS |
| **3389** | RDP (Remote Desktop Protocol) | Log in to Windows EC2 instances |

---

## Details

### SSH (Port 22)

Used to securely connect to Linux servers.

Example:

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

**Common AWS Use Case:**
- Managing Linux EC2 instances

---

### FTP (Port 21)

Used for file transfers between systems.

**Note:**
FTP is not encrypted and is generally discouraged for sensitive data.

**Common AWS Use Case:**
- Legacy file transfer systems

---

### SFTP (Port 22)

Secure version of FTP that runs over SSH.

**Benefits:**
- Encrypted communication
- More secure than FTP

**Common AWS Use Case:**
- Secure file uploads and downloads

---

### HTTP (Port 80)

Standard protocol for unencrypted web traffic.

Example:

```text
http://example.com
```

**Common AWS Use Case:**
- Public websites
- Load balancers
- Web applications

---

### HTTPS (Port 443)

Secure version of HTTP using SSL/TLS encryption.

Example:

```text
https://example.com
```

**Common AWS Use Case:**
- Secure websites
- APIs
- Online applications

**Exam Tip:**  
When security or encryption is mentioned, expect traffic on **port 443**.

---

### RDP (Port 3389)

Remote Desktop Protocol used to connect to Windows servers.

**Common AWS Use Case:**
- Managing Windows EC2 instances

Example:

```text
Your Computer
      ↓
RDP (3389)
      ↓
Windows EC2 Instance
```

---

## Quick Exam Memorization

| Port | Remember As |
|--------|-------------|
| **21** | FTP |
| **22** | SSH / SFTP |
| **80** | HTTP |
| **443** | HTTPS |
| **3389** | Windows RDP |

### Easy Memory Trick

- **22** → Linux
- **80** → Website
- **443** → Secure Website
- **3389** → Windows Desktop
- **21** → File Transfer

# How to SSH into Your EC2 Instance

SSH is one of the most important functions. It allows you to control a remote machine, all using the command line.


# EC2 Instance Connect

- Connect to your EC2 instance directly from your web browser.
- No need to use the downloaded key pair (`.pem` file).
- AWS temporarily uploads a key to the EC2 instance to authenticate your session.
- Works out of the box only with **Amazon Linux 2**.
- **Port 22 (SSH)** must still be open in the Security Group.

---

# EC2 Purchase Options

## On-Demand Instances

- Best for short-term workloads.
- Predictable pricing.
- Pay only for what you use (billed per second).

---

## Reserved Instances (1 or 3 Years)

Designed for long-running workloads.

### Reserved Instances

- Commit to a 1- or 3-year term.
- Lower cost compared to On-Demand.

### Convertible Reserved Instances

- Commit to a 1- or 3-year term.
- Allows flexibility to change instance types during the reservation.

---

## Savings Plans (1 or 3 Years)

- Commit to a certain amount of compute usage.
- Best for long-term workloads.
- Provides flexible pricing discounts.

---

## Spot Instances

- Best for short-term workloads.
- Significantly cheaper than On-Demand.
- AWS can reclaim the instance at any time.
- Less reliable for critical applications.

---

## Dedicated Hosts

- Reserve an entire physical server.
- Full control over instance placement.
- Useful for licensing and compliance requirements.

---

## Dedicated Instances

- Your instances run on hardware that is not shared with other AWS customers.
- Less control than Dedicated Hosts, but provides hardware isolation.

---

## Capacity Reservations

- Reserve compute capacity in a specific Availability Zone (AZ).
- Can be reserved for any duration.
- Ensures capacity is available when needed.


# Elastic IP (EIP)

By default, when you **stop** and then **start** an EC2 instance, its **public IP address may change**.

If you need a permanent public IP address, you can use an **Elastic IP (EIP)**.

## Key Characteristics

- An Elastic IP is a **static public IPv4 address**.
- You own the IP until you explicitly release (delete) it.
- An Elastic IP can be attached to **only one EC2 instance at a time**.
- You can quickly remap an Elastic IP from one instance to another, making it useful for failover scenarios.

## Limits

- By default, each AWS account can have **up to 5 Elastic IP addresses**.
- You can request a quota increase from AWS if needed.

## Best Practices

Try to avoid using Elastic IPs whenever possible because they often indicate poor architecture.

Instead, consider:

- Using a dynamic public IP with a DNS record pointing to it.
- Using an **Elastic Load Balancer (ELB)** so clients connect to the load balancer instead of directly to an EC2 instance.

---

# Placement Groups

Placement Groups allow you to control **how EC2 instances are physically placed** within AWS infrastructure.

There are three placement strategies.

## 1. Cluster Placement Group

- Places instances close together.
- All instances are located in a **single Availability Zone (AZ)**.
- Provides **low network latency** and **high network throughput**.

### Best For

- High-performance computing (HPC)
- Big data workloads
- Applications requiring very fast communication between instances

---

## 2. Spread Placement Group

- Places instances on **different underlying hardware**.
- Reduces the risk of multiple instances failing due to hardware failure.
- Maximum of **7 instances per Availability Zone**.

### Best For

- Critical applications where high availability is important.

---

## 3. Partition Placement Group

- Divides instances across multiple **partitions**.
- Each partition uses a different set of racks and hardware.
- Can scale to **hundreds of EC2 instances**.

### Best For

Large distributed systems such as:

- Hadoop
- Cassandra
- Kafka

---

# Elastic Network Interface (ENI)

An **Elastic Network Interface (ENI)** is a virtual network card that exists inside a **Virtual Private Cloud (VPC)**.

## An ENI Can Have

- One primary private IPv4 address
- One or more secondary private IPv4 addresses
- One Elastic IP for each private IPv4 address
- One public IPv4 address
- One or more Security Groups
- A MAC address

## Features

- Can be created independently of an EC2 instance.
- Can be attached or detached from EC2 instances at any time.
- Useful for failover by moving the network interface to another instance.
- Bound to a specific **Availability Zone (AZ)**.

---

# EC2 Hibernate

Normally, EC2 instances support two lifecycle actions.

## Stop

- Instance shuts down.
- Root EBS volume is preserved.
- RAM contents are lost.

## Terminate

- Instance is permanently deleted.
- Root EBS volume is deleted (if configured to do so).
- All RAM contents are lost.

## What Happens During Startup?

### First Boot

- Operating system boots.
- EC2 User Data script runs.
- Applications start.

### Later Boots

- Operating system boots.
- Applications start.
- Caches are rebuilt and services initialize, which can take time.

---

# EC2 Hibernate

Hibernate allows an EC2 instance to resume exactly where it left off.

Instead of shutting down completely:

- The contents of RAM are saved to the encrypted root EBS volume.
- The operating system is not fully restarted.
- Applications continue from the previous state.

## Advantages

- Much faster startup time.
- Preserves the contents of memory (RAM).
- Avoids lengthy application initialization.

## Under the Hood

- RAM is written to a file on the **encrypted root EBS volume**.
- The root volume **must** be an encrypted EBS volume.

## Common Use Cases

- Long-running computations
- Preserving application state in memory
- Applications that require significant startup time

---

# EC2 Hibernate Requirements

## Supported Instance Families

Examples include:

- C3, C4, C5
- I3
- M3, M4
- R3, R4
- T2, T3
- And many newer supported families

## Requirements

- RAM must be **less than 150 GB**.
- Bare metal instances are **not supported**.
- Supported operating systems include:
  - Amazon Linux 2
  - Linux AMIs
  - Ubuntu
  - RHEL
  - CentOS
  - Windows
- Root volume must:
  - Be an encrypted EBS volume
  - Not be an Instance Store volume
  - Be large enough to store the RAM contents

## Pricing Options Supported

Hibernate works with:

- On-Demand Instances
- Reserved Instances
- Spot Instances

## Limitation

An EC2 instance **cannot remain hibernated for more than 60 days**.
