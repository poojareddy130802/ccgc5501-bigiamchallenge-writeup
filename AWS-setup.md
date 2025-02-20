

## Reflection on Setting Up My Personal AWS Environment

### Steps Taken

1. **Created an AWS Account:**
   I started by creating a new AWS account to begin configuring my cloud environment.

2. **Enabled MFA for the Root User:**
   I immediately enabled Multi-Factor Authentication (MFA) for the root user using Microsoft Authenticator. This ensures that even if someone gets hold of my root credentials, they would still need access to my phone to log in.

3. **Created an Administrator User:**
   I created a new user with Administrator Access to avoid using the root account for day-to-day tasks. This follows AWS best practices for securing the account.

4. **Configured Admin Password Reset:**
   I enabled the option to reset the admin password on the next login. This ensures that I can regain control of the account if there are any issues with my access.

5. **Enabled MFA for the Administrator User:**
   To further enhance security, I enabled MFA for the administrator user as well. This adds an additional layer of protection to the account, ensuring only authorized access.

### Understanding Key AWS Policies
One of the things I looked on to was the `AdministratorAccess` policy, which grants access to all 437 services in AWS. This policy ensures the user has full permissions to manage AWS services, making it ideal for users responsible for managing resources at a high level. So any user with this role would still be able to do all actions in AWS and no need to provide the root user credentials.

### Challenges Faced

- **New to AWS:**
  Being new to AWS, I found the user interface (UI) a bit overwhelming at first. The sheer number of options and features made it difficult to know where to begin and how to navigate through the setup process.
  
- **Assigning Admin Role to a User:**
  I also faced a challenge when I was trying to assign the "Administrator" role to a user. The IAM (Identity and Access Management) interface wasn’t immediately intuitive to me, and I wasn’t sure how to properly assign the necessary permissions.

### How I Overcame the Challenges

- **Referred to the AWS Documentation:**
  When I encountered difficulties, I referred to the official AWS documentation. This resource provided detailed, step-by-step guidance that helped me understand how to assign the admin role to a user and navigate the IAM interface effectively. The documentation was key in clearing up any confusion and building my confidence in working with AWS.

### Security Measures Implemented

- **Multi-Factor Authentication (MFA):**
  I set up MFA for both the root and administrator accounts. This protects my account from unauthorized access by requiring both my password and a second factor (my phone).

- **Administrator User (Not Using Root for Daily Tasks):**
  I created an admin user to ensure I follow the principle of least privilege. By using an admin user for regular tasks, I reduce the risk of compromising the root user.

- **Password Reset for Admin:**
  The ability to reset the admin password on the next login ensures I can easily recover access if needed, adding another layer of security.

### Why do we even need an "admin" user when you have access to the root user?
  - The root user should only be used for critical account-related tasks to minimize security risks.
  - Using an admin user for everyday tasks follows the principle of least privilege, which reduces the potential impact if the account is compromised.
  - The root user has broad access to sensitive settings, so limiting its use helps protect the integrity of the account.

---

