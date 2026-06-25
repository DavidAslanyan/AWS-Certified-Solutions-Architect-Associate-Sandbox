
# AWS Certified Solutions Architect – Associate Exam Prep

## How to Choose an AWS Region

When selecting an AWS Region, consider:

- **Compliance** – Data governance and legal requirements.
- **Proximity to Customers** – Reduces latency and improves performance.
- **Available Services** – Not all AWS services are available in every region.
- **Pricing** – AWS pricing varies between regions.

---

## AWS Availability Zones (AZs)

- Each AWS Region contains multiple Availability Zones.
  - Minimum: **3 AZs**
  - Maximum: **6 AZs**
- Each AZ is a discrete data center with:
  - Independent power
  - Independent networking
  - Independent connectivity
- AZs are physically separated to reduce the impact of disasters.
- AZs are connected through:
  - High-bandwidth networking
  - Ultra-low-latency links

---

## IAM (Identity and Access Management)

**IAM** is a global AWS service used to manage access to AWS resources.

### Key Concepts

- AWS creates a **Root Account** by default.
  - Should not be used for daily tasks.
  - Should never be shared.

- **Users**
  - Represent individual people within an organization.
  - Can be assigned permissions directly.
  - Can belong to multiple groups.
  - Can belong to no groups.

- **Groups**
  - Used to manage permissions for multiple users.
  - Can contain only users.
  - Cannot contain other groups.

### Policies

- Users and groups can be assigned **IAM Policies**.
- Policies are JSON documents that define permissions.
- Policies specify:
  - What actions are allowed or denied.
  - Which AWS resources can be accessed.

### Security Best Practice

#### Principle of Least Privilege

Grant users only the permissions they need to perform their job and nothing more.

**Example:**
- A developer should have access only to the services they need.
- An accountant should not have EC2 administration permissions.
- An intern should not have full administrator access.


# IAM Setting Protection for Login

For your AWS account, you should have password policies in place.

You can configure password policies for IAM users to:

- Require a minimum number of characters
- Require special characters, letters, and numbers
- Force password changes after a specified period of time
- Prevent users from reusing old passwords when changing them

---

# IAM Roles for Services

In AWS, some services need to perform actions on your behalf.

To allow this, you assign IAM Roles with specific permissions to those services so they can securely access AWS resources and perform required operations.

## Common IAM Roles

- EC2 Instance Roles
- Lambda Function Roles
- CloudFormation Roles

### Example

You may have an EC2 instance (virtual server) that needs to perform actions within AWS, such as reading from an S3 bucket.

To enable this, you assign an IAM Role to the EC2 instance with the necessary permissions.

---

# IAM Security Tools

## IAM Credentials Report (Account Level)

A report that lists all IAM users in your account and the status of their various credentials, such as:

- Passwords
- Access keys
- MFA devices

## IAM Access Advisor / Last Accessed (User Level)

Access Advisor shows:

- The service permissions granted to a user
- When those services were last accessed

You can use this information to review and refine IAM policies.

---

# IAM Guidelines & Best Practices

- Don't use the root account except for AWS account setup and critical administrative tasks.
- One physical user = One AWS user.
- Assign users to groups and assign permissions to groups.
- Create a strong password policy.
- Use and enforce Multi-Factor Authentication (MFA).
- Create and use IAM Roles when granting permissions to AWS services.
- Use Access Keys for programmatic access (CLI / SDK).
- Audit permissions using:
  - IAM Credentials Report
  - IAM Access Advisor
- Never share IAM users or Access Keys.
- Prevent password reuse when users change their passwords.

---

# Multi-Factor Authentication (MFA)

A strong password alone is not sufficient to protect your root account and IAM users.

You should also enable Multi-Factor Authentication (MFA).

## What is MFA?

MFA = Something you know + Something you own

- Something you know: Your password
- Something you own: A security device or authenticator

AWS allows you to add up to **8 MFA devices** per user.

Using MFA significantly improves account security by requiring an additional verification step during login.


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
