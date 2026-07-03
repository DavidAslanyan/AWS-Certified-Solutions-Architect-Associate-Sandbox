
# RDS – Storage Auto Scaling

RDS Storage Auto Scaling automatically increases the storage allocated to your RDS database when it detects that the database is running out of free space.

## Benefits

- Dynamically increases database storage.
- Eliminates the need for manual storage scaling.
- Helps prevent storage-related outages.

## Requirements

You must configure a **Maximum Storage Threshold**, which defines the maximum storage size that RDS is allowed to allocate.

## Automatic Scaling Conditions

RDS automatically increases storage when all of the following conditions are met:

- Free storage is less than **10%** of the allocated storage.
- Low storage condition lasts for at least **5 minutes**.
- At least **6 hours** have passed since the last storage modification.

## Use Cases

- Applications with unpredictable workloads.
- Supports all Amazon RDS database engines.

---

# RDS Read Replicas

Read Replicas improve read performance by creating one or more read-only copies of your database.

## Features

- Supports up to **15 Read Replicas**.
- Can be created:
  - Within the same Availability Zone (AZ)
  - Across different AZs
  - Across different AWS Regions
- Replication is **asynchronous (ASYNC)**.
- Read replicas provide **eventual consistency**.
- Read Replicas can be promoted to become standalone databases.

## Notes

- **Writes** always go to the primary database.
- **Reads** can be distributed across Read Replicas.
- Applications must be configured to send read traffic to the replicas.

---

# RDS – From Single-AZ to Multi-AZ

You can convert a Single-AZ RDS deployment to a Multi-AZ deployment with **zero downtime**.

## How to Enable

- Select the RDS instance.
- Choose **Modify**.
- Enable **Multi-AZ Deployment**.

## What Happens Internally

AWS performs the following steps automatically:

1. Takes a snapshot of the primary database.
2. Restores a new database from the snapshot in another Availability Zone.
3. Establishes continuous synchronization between the primary and standby databases.

---

# RDS Custom

RDS Custom provides a managed Oracle or Microsoft SQL Server database while allowing operating system and database-level customization.

## Standard RDS

AWS manages:

- Database installation
- Operating system
- Patching
- Backups
- Scaling
- Maintenance

## RDS Custom

Provides administrator access to both the operating system and the database.

You can:

- Configure OS and database settings.
- Install custom patches.
- Enable native database features.
- Access the underlying EC2 instance using:
  - SSH
  - AWS Systems Manager (SSM) Session Manager

## Customization

To perform custom changes:

- Disable **Automation Mode**.
- It is recommended to create a database snapshot before making modifications.

## RDS vs RDS Custom

| Feature | RDS | RDS Custom |
|---------|-----|------------|
| OS Managed by AWS | ✅ | ❌ |
| Database Managed by AWS | ✅ | Partial |
| OS Access | ❌ | ✅ |
| SSH Access | ❌ | ✅ |
| Database Customization | Limited | Full |
| Best For | Standard managed databases | Oracle & SQL Server workloads requiring OS/database customization |

# Amazon Aurora

Amazon Aurora is a proprietary relational database engine developed by AWS.

## Overview

- Aurora is an AWS proprietary technology (not open source).
- Compatible with:
  - PostgreSQL
  - MySQL
- Existing PostgreSQL and MySQL drivers work with Aurora without modification.
- Optimized specifically for the AWS Cloud.

## Performance

- Up to **5×** faster than MySQL running on Amazon RDS.
- Up to **3×** faster than PostgreSQL running on Amazon RDS.

## Storage

- Storage automatically grows in **10 GB increments**.
- Supports up to **256 TB** of storage.

## Replication

- Supports up to **15 Aurora Replicas**.
- Replication is much faster than standard MySQL replication.
- Typical replica lag is **less than 10 ms**.

## High Availability

- Instantaneous failover.
- High Availability (HA) is built into Aurora.

## Pricing

- Aurora costs approximately **20% more** than Amazon RDS.
- The additional cost is justified by higher performance and greater efficiency.

---

# Features of Amazon Aurora

- Automatic failover
- Backup and recovery
- Isolation and security
- Industry compliance
- Push-button scaling
- Automated patching with zero downtime
- Advanced monitoring
- Routine maintenance
- **Backtrack** – restore the database to a previous point in time without using backups
