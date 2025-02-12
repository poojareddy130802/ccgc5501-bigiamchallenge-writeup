# Challenge 4

## Challenge Statement
**Admin only?** We learned from our mistakes from the past. Now our bucket only allows access to one specific admin user. Or does it?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?

I have access to list files in the files/ directory under the condition that the request is made by the admin user. Additionally, anyone can retrieve objects in the bucket using s3:GetObject.

* What don't I have access to?

While I can list files under the files/ prefix, I’m not able to access any objects that are not under that prefix or perform actions outside of s3:GetObject or s3:ListBucket.

* What do I find interesting?

The condition block is interesting because it suggests that only the admin user should be able to list objects in the files/ directory, but it still allows everyone to get objects without restrictions. This creates an opportunity for unintended access via other means.

* etc

Based on what I understood from the IAM Policy, the policy defines two main rules:

* The first statement allows anyone (Principal: *) to perform the s3:GetObject action on any objects in the thebigiamchallenge-admin-storage-abf1321 bucket. This means that objects can be read by anyone.

* The second statement allows anyone to list objects in the bucket, but it restricts this access using the Condition block. The condition ensures that the action is only allowed if the principal (the user or entity making the request) is specifically the admin user (arn:aws:iam::133713371337:user/admin). Additionally, the listing is limited to objects under the files/* prefix.
```

## Solution
* Initially, I ran the command to list the contents of the files/ directory using the aws s3 ls command:

```bash
aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ --no-sign-request
```

This command listed the contents, including the flag-as-admin.txt file.

* To view the contents of the flag, I copied it by running:

```bash
aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt - --no-sign-request
```

The command successfully retrieved the content of the flag.

* I copied the flag text and pasted it in the portal to complete the challenge.

## Reflection
```
* What was your approach?

I initially thought that the solution might involve accessing the admin's credentials directly, but when that didn’t work, I focused on the documentation and tried to understand the IAM policy more deeply. I then realized that the --no-sign-request flag might bypass IAM conditions and allow unauthenticated access to the file.

* What was the biggest challenge?

The biggest challenge was figuring out the role of the --no-sign-request flag. Initially, I couldn't figure out how to bypass the IAM policy restrictions, so I spent time looking for admin credentials or other clues without success.

* How did you overcome the challenges?

By researching AWS documentation, I learned that the --no-sign-request flag allows requests to be made without signing them, which effectively bypasses the IAM conditions. This led to my breakthrough.

* What led to the break through?

The breakthrough came when I read about the --no-sign-request flag in the AWS CLI documentation. I understood that this flag would allow me to access resources without signing the request, which was key in bypassing the condition restricting access to only the admin.

* On the blue side, how can the learning be used to properly defend the important assets? 

This exercise highlights the importance of reviewing IAM policies carefully. To properly defend assets, ensure that conditions and permissions are properly scoped and not overly permissive. In this case, allowing s3:GetObject access to everyone could potentially expose sensitive data. Proper access controls, such as limiting s3:GetObject permissions or utilizing signed URLs, would be more secure practices for protecting sensitive resources.
```