# Amazon EBS (Elastic Block Store)

An **Amazon EBS (Elastic Block Store) Volume** is a network-attached storage device that can be connected to an EC2 instance.

It provides persistent storage, meaning your data remains available even if the EC2 instance is stopped or terminated (provided the volume is not configured to be deleted).

> **Analogy:** Think of an EBS volume as a **network-attached USB drive** for your EC2 instance.

---

# EBS Volume Characteristics

## Persistent Storage

- Data persists independently of the EC2 instance.
- Can be detached from one instance and attached to another.

## Network Storage

- EBS is a **network drive**, not a physical disk.
- Communication between the EC2 instance and the volume occurs over the AWS network.
- May introduce slight network latency.

## Availability Zone (AZ) Bound

- An EBS volume belongs to a single Availability Zone.
- It can only be attached to EC2 instances within the same AZ.

For example:

- An EBS volume in **us-east-1a** cannot be attached directly to an instance in **us-east-1b**.

To move an EBS volume to another AZ or Region:

1. Create a snapshot.
2. Restore the snapshot into the target AZ or Region.

## Capacity

EBS volumes are provisioned with:

- Storage size (GB)
- IOPS (Input/Output Operations Per Second)

You are billed based on the provisioned capacity, not actual usage.

The storage capacity can be increased later if needed.

---

# EBS Snapshots

An **EBS Snapshot** is a point-in-time backup of an EBS volume.

## Characteristics

- Used for backups and disaster recovery.
- The volume does **not** need to be detached before creating a snapshot, although doing so is recommended for data consistency.
- Snapshots can be copied across:
  - Availability Zones (by restoring them)
  - AWS Regions

Snapshots are commonly used for:

- Backups
- Cloning volumes
- Migrating storage between Availability Zones or Regions

---

# Amazon Machine Image (AMI)

An **Amazon Machine Image (AMI)** is a preconfigured template used to launch EC2 instances.

An AMI contains everything needed to start an instance, including:

- Operating system
- Installed software
- System configuration
- Monitoring agents
- Custom applications

Because everything is preconfigured, launching instances from an AMI results in much faster deployments.

## AMI Scope

AMIs are regional resources.

They can be copied to other AWS Regions if needed.

## Types of AMIs

### Public AMIs

Provided and maintained by AWS.

### Custom AMIs

Created and managed by you.

### AWS Marketplace AMIs

Created by third parties and available through the AWS Marketplace (some are paid).

---

# EC2 Instance Store

An **EC2 Instance Store** is temporary storage physically attached to the host machine running your EC2 instance.

Unlike EBS, it is **not network-attached**.

## Characteristics

- Very high I/O performance
- Hardware-based storage
- Data is lost if the instance is stopped or terminated
- Data may also be lost if the underlying hardware fails

Because of its temporary nature, Instance Store is best used for:

- Buffers
- Caches
- Scratch data
- Temporary files

Backups and replication are entirely your responsibility.

---

# EBS Volume Types

Amazon EBS provides six volume types.

## SSD Volumes

### gp2 / gp3

General Purpose SSD volumes.

- Balanced price and performance
- Suitable for most applications
- Common default choice

### io1 / io2 Block Express

Provisioned IOPS SSD volumes.

- Highest performance
- Lowest latency
- Designed for mission-critical applications and databases

---

## HDD Volumes

### st1

Throughput Optimized HDD.

- Lower cost
- Designed for frequently accessed, throughput-intensive workloads

### sc1

Cold HDD.

- Lowest cost
- Designed for infrequently accessed data

---

# EBS Performance Metrics

EBS volumes are characterized by:

- Storage Size (GB)
- Throughput
- IOPS (Input/Output Operations Per Second)

> When selecting a volume type, always consult the latest AWS documentation for current performance limits and recommendations.

## Boot Volume Support

Only the following volume types can be used as boot (root) volumes:

- gp2
- gp3
- io1
- io2 Block Express

---

# EBS Multi-Attach

**EBS Multi-Attach** allows a single EBS volume to be attached to multiple EC2 instances simultaneously.

## Requirements

- Supported only on **io1** and **io2** volumes.
- All EC2 instances must be in the **same Availability Zone**.
- Supports up to **16 EC2 instances** simultaneously.

## Characteristics

- Every attached instance has read and write access.
- Applications must handle concurrent writes correctly.
- Requires a cluster-aware file system.

Examples of unsupported file systems:

- XFS
- EXT4

## Common Use Case

Clustered Linux applications requiring shared high-performance storage, such as:

- Teradata

---

# EBS Encryption

Amazon EBS supports encryption using **AWS Key Management Service (KMS)** with **AES-256** encryption.

When an EBS volume is encrypted:

- Data stored on the volume (data at rest) is encrypted.
- Data transferred between the EC2 instance and the volume (data in transit) is encrypted.
- Snapshots are automatically encrypted.
- Any new volumes created from encrypted snapshots are also encrypted.

## Additional Notes

- Encryption and decryption happen automatically.
- No application changes are required.
- Encryption has only a minimal impact on performance.

## Encryption Scenarios

- You can create an encrypted copy of an unencrypted snapshot.
- Snapshots created from encrypted volumes remain encrypted.
