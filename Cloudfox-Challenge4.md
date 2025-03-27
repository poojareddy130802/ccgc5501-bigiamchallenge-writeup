## Solution

### Step 1: **Create a New Profile to Assume the `ertz` Role**

- Edit the AWS configuration file to add the new profile for assuming the `ertz` role.

```bash
vim ~/.aws/config
```

- Add the following configuration:

```ini
[profile ertz]
region = us-west-2
role_arn = arn:aws:iam::477892879619:role/Ertz
source_profile = cloudfoxable
```

This sets up a profile called `ertz` that will allow you to assume the `Ertz` role with the `cloudfoxable` profile as the source.

---

### Step 2: **Verify the Role Assumption**

- Use the AWS CLI to verify that the profile works and check the identity of the assumed role:

```bash
aws --profile ertz sts get-caller-identity
```

- Expected output:

```json
{
    "UserId": "AROAQWHCQAQBTDUKGU3W5:botocore-session-1743032685",
    "Account": "477892879619",
    "Arn": "arn:aws:sts::477892879619:assumed-role/Ertz/botocore-session-1743032685"
}
```

This confirms that the `ertz` profile is correctly assuming the `Ertz` role.

---

### Step 3: **Check Role Permissions**

- Use the `cloudfox` tool to check the permissions associated with the `ertz` role. You will want to verify which actions the role can perform.

```bash
cloudfox aws -p cloudfoxable permissions --principal ertz -v2
```

- The output will show the role permissions, including access to the SSM parameter:

```text
Type │ Name │ Policy  │        Policy Name        │ Effect │      Action      │                                     Resource                                      │ Condition │
├──────┼──────┼─────────┼───────────────────────────┼────────┼──────────────────┼───────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ Role │ Ertz │ Managed │ its-another-secret-policy │ Allow  │ ssm:GetParameter │ arn:aws:ssm:us-west-2:477892879619:parameter/cloudfoxable/flag/its-another-secret │ No
```

This confirms that the `ertz` role has permission to access the SSM parameter `/cloudfoxable/flag/its-another-secret`.

---

### Step 4: **Retrieve the Secret from SSM Parameter Store**

- With the permissions verified, you can now retrieve the secret value using the following AWS CLI command:

```bash
aws --profile ertz --region us-west-2 ssm get-parameter --with-decryption --name /cloudfoxable/flag/its-another-secret
```

- Expected output:

```json
{
    "Parameter": {
        "Name": "/cloudfoxable/flag/its-another-secret",
        "Type": "SecureString",
        "Value": "FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}",
        "Version": 1,
        "LastModifiedDate": "2025-02-12T09:26:39.812000-05:00",
        "ARN": "arn:aws:ssm:us-west-2:477892879619:parameter/cloudfoxable/flag/its-another-secret",
        "DataType": "text"
    }
}
```

The flag value is `FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}`.

---

## Reflection

- **What was your approach?**  
  I set up a new profile to assume the `ertz` role, verified the role assumption, checked permissions for the role, and then retrieved the secret from the SSM parameter store using the AWS CLI.

- **What was the biggest challenge?**  
  The biggest challenge was figuring out which permissions were granted to the `ertz` role. Once I understood that the role had access to the SSM parameter, I was able to move forward.

- **How did you overcome the challenges?**  
  By using the `cloudfox` tool to review the permissions and ensuring I had the correct permissions for accessing the secret, I was able to proceed with retrieving the flag.

- **What led to the breakthrough?**  
  The breakthrough came when I realized that the role had specific permissions for accessing the SSM parameter, which directly led me to the flag.

- **On the blue side, how can the learning be used in the future?**  
  This experience taught me how to properly assume roles and inspect permissions. In the future, this knowledge will help me navigate role-based access controls and work with sensitive data securely within AWS. It also emphasized the importance of understanding AWS CLI commands for retrieving data from services like SSM.