
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
