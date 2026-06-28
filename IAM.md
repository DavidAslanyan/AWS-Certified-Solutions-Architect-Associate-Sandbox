
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


