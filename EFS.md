# Amazon EFS (Elastic File System)

**Amazon Elastic File System (EFS)** is a fully managed **Network File System (NFS)** that provides shared file storage for multiple EC2 instances.

Unlike EBS, which is typically attached to a single EC2 instance, EFS can be mounted simultaneously by **many EC2 instances**, even across multiple Availability Zones.

---

# EFS Features

## Shared File System

- Multiple EC2 instances can mount the same file system simultaneously.
- Works across multiple Availability Zones (Multi-AZ).
- Ideal for applications that need shared storage.

## Fully Managed

AWS automatically handles:

- Scaling storage capacity
- High availability
- Durability
- Maintenance

There is **no capacity planning** required—you simply pay for the storage you use.

## Protocol

EFS uses the **NFS v4.1 (Network File System)** protocol.

## Security

- Access is controlled using **Security Groups**.
- Supports encryption at rest using **AWS KMS**.
- Supports POSIX file permissions (standard Linux file permissions).

## Operating System Support

- Compatible with **Linux-based AMIs**.
- **Not supported on Windows EC2 instances.**

---

# Common Use Cases

Amazon EFS is commonly used for:

- Content Management Systems (CMS)
- Web servers
- Shared application data
- WordPress hosting
- Data sharing between multiple EC2 instances

---

# Benefits

- Highly available
- Automatically scales as files are added or removed
- Shared storage across many EC2 instances
- Multi-AZ support
- Pay only for what you use

> **Note:** EFS is significantly more expensive than General Purpose EBS volumes (approximately three times the cost of gp2 in many cases).

---

# EBS vs EFS

| Feature | EBS | EFS |
|---------|-----|-----|
| Storage Type | Block Storage | Shared File Storage |
| Can Attach To | One EC2 instance (except Multi-Attach io1/io2) | Hundreds of EC2 instances |
| Multi-AZ Support | ❌ No | ✅ Yes |
| Availability Zone Scope | Single AZ | Multiple AZs |
| Operating Systems | Linux & Windows | Linux only |
| Protocol | Block Device | NFS v4.1 |
| Capacity | Fixed (can be increased manually) | Automatically scales |
| Pricing | Lower | Higher |
| Best Use Cases | Databases, operating systems, single-server applications | Shared websites, WordPress, file sharing |

---

# Important EBS Notes

## Availability Zone Restriction

EBS volumes are locked to a single Availability Zone.

To move an EBS volume to another AZ:

1. Create a snapshot.
2. Restore the snapshot into the target Availability Zone.

## Performance

### gp2

- Performance (IOPS) increases as disk size increases.

### gp3 and io1/io2

- Storage size and IOPS can be configured independently.

## Snapshots

Creating EBS snapshots consumes I/O resources.

Avoid taking snapshots while your application is handling heavy production traffic whenever possible.

---

# Important EFS Notes

## Multi-Instance Access

EFS allows **hundreds of Linux EC2 instances** across multiple Availability Zones to access the same shared files simultaneously.

A common example is a WordPress deployment where multiple web servers all serve the same website files.

## Storage Tiers

EFS supports storage tiers that can reduce costs for files that are accessed less frequently.

---

# EBS vs EFS vs Instance Store

| Feature | EBS | EFS | Instance Store |
|---------|-----|-----|----------------|
| Storage Type | Network Block Storage | Shared Network File System | Local Physical Storage |
| Persistent | ✅ Yes | ✅ Yes | ❌ No |
| Shared Between Instances | No (except Multi-Attach) | Yes | No |
| Multi-AZ | No | Yes | No |
| Performance | High | High | Very High |
| Best For | Operating systems, databases | Shared application files | Cache, temporary files, scratch data |
| Data Survives Instance Stop | ✅ Yes | ✅ Yes | ❌ No |

---

# When to Use Which?

### Use **EBS** when:

- You need storage for a single EC2 instance.
- You need a boot volume.
- You are running databases or applications requiring block storage.

### Use **EFS** when:

- Multiple EC2 instances need access to the same files.
- You need a scalable shared file system.
- You are hosting applications like WordPress or content management systems.

### Use **Instance Store** when:

- Maximum disk performance is required.
- The data is temporary and can be recreated.
- You need storage for caches, buffers, or scratch files.
