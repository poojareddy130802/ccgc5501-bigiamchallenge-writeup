# Cloudfoxable - 3: Its a Secret

## Solution

### Step 1: **Set AWS Profile**
First, I set the AWS profile to ensure all subsequent commands use the correct profile. I ran:
```bash
export AWS_PROFILE=cloudfoxable
```

### Step 2: **Confirm User and Profile**
Next, I confirmed the user and profile to ensure that I was using the correct identity. I ran:
```bash
aws sts get-caller-identity
```
Output:
```json
{
    "UserId": "AIDAQWHCQAQBFYP4I5B2",
    "Account": "477892879619",
    "Arn": "arn:aws:iam::477892879619:user/ctf-starting-user"
}
```
The output confirmed that I was logged in as `ctf-starting-user` in the correct account.

### Step 3: **Check for Secrets Using Cloudfox**
I then used Cloudfox to list available secrets that might contain valuable information. I ran:
```bash
cloudfox aws secrets -v2
```
Output:
```
|    Service     │  Region   │                 Name                  │           Description            
├────────────────┼───────────┼───────────────────────────────────────┼──────────────────────────────────
│ SecretsManager │ us-west-2 │ database-secret                       │                                  
│ SecretsManager │ us-west-2 │ SegueFlag                             │                                  
│ SecretsManager │ us-west-2 │ DomainAdministrator-Credentials       │                                  
│ SecretsManager │ us-west-2 │ my-app-secret                         │ Secure secret for sensitive data 
│ SSM            │ us-west-2 │ /cloudfoxable/flag/executioner        │                                  
│ SSM            │ us-west-2 │ /cloudfoxable/flag/its-a-secret       │                                  
│ SSM            │ us-west-2 │ /cloudfoxable/flag/its-another-secret │                                  
│ SSM            │ us-west-2 │ /cloudfoxable/flag/lambda-sqs         │                                  
```
I noticed that `/cloudfoxable/flag/its-a-secret` was listed under SSM, which seemed like the potential source for the flag.

### Step 4: **Review Secrets with Cloudfox Output**
Cloudfox saved the list of commands needed to retrieve secrets. I reviewed the saved file to see the necessary commands:
```bash
cat /home/n01689364/.cloudfox/cloudfox-output/aws/cloudfoxable-477892879619/loot/pull-secrets-commands.txt
```

### Step 5: **Check User Permissions**
Before attempting to retrieve the secret, I checked the permissions of `ctf-starting-user` to ensure I had the necessary access. I ran the following command:
```bash
cloudfox aws permissions --principal ctf-starting-user -v2
```
Output:
```
╭──────┬───────────────────┬─────────┬───────────────────────┬────────┬─────────────────────────────────────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────┬───────────╮
│ Type │       Name        │ Policy  │      Policy Name      │ Effect │                             Action                              │                                  Resource                                   │ Condition │
├──────┼───────────────────┼─────────┼───────────────────────┼────────┼─────────────────────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────┼───────────┤
│ User │ ctf-starting-user │ Managed │ its-a-secret-policy   │ Allow  │ ssm:GetParameter                                                │ arn:aws:ssm:us-west-2:477892879619:parameter/cloudfoxable/flag/its-a-secret │ No 
```
The output confirmed that the user had the necessary `ssm:GetParameter` permission for `/cloudfoxable/flag/its-a-secret`.

### Step 6: **Retrieve the Secret Value**
With the required permissions confirmed, I proceeded to retrieve the secret. I ran the following command:
```bash
aws ssm get-parameter --with-decryption --name /cloudfoxable/flag/its-a-secret
```
Output:
```
{
    "Parameter": {
        ...
        "Value": "FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}",
        ...
    }
}
```
The flag value was retrieved from the parameter value.

---

## Reflection

- **What was your approach?**  
  I followed a structured process by confirming the user identity, searching for secrets, verifying permissions, and then retrieving the secret.

- **What was the biggest challenge?**  
  The biggest challenge was confirming that the user had the correct permissions to access the secret.

- **How did you overcome the challenges?**  
  I used the `cloudfox aws permissions` command to verify the user's permissions before attempting to retrieve the secret.

- **What led to the breakthrough?**  
  The breakthrough came after confirming the permission and executing the `get-parameter` command, successfully retrieving the secret.

- **On the blue side, how can the learning be used in future?**  
  Understanding the importance of verifying permissions and securing sensitive data in cloud environments will be helpful for managing AWS resources securely in the future.
