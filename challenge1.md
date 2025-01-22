# Challenge 1

## Challenge Statement
Buckets of Fun - We all know that public buckets are risky. But can you find the flag?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?

1. I have access to view / download any objects in that S3 Bucket
2. Access to list the objects inside files directory

* What don't I have access to?

1. Write / Update / Delete anywhere in the S3 bucket
2. List the files in the root directory

* What do I find interesting?

Two things are interesting, one is obviously the *, which gives access to everyone and anyone on the internet
They have restricted the list access for the root directory but given read access.

* etc
```

## Solution
* Access the S3 bucket through AWS CLI
* List the files in the /files directory using ls command
* Read the contents of the flag1.txt file
* Copy the Flag text and paste it in the portal

## Reflection
```
* What was your approach?

My approach was to first get commands for accessing the remote public repository.
As I listed, I was able to find the flag1.txt file being present in the S3 bucket
To read the contents of a file in the S3 bucket, used the cp command with -

* What was the biggest challenge?

Viewing the file contents was a bigger chanllenge. First did cat command, but the command was not in the possible options, tried many. It took more time to find the cp command with hyphen. I tried dot but it was read only terminal, so even that threw error. It was all new for me, the AWS, AWS CLI and the IAM policy.


* How did you overcome the challenges?

I was well versed in using the help command and finding options through which cp command was the closest option to read the contents of the file.
When surfed on the possible options to display the contents of the file in the current terminal, I got the hyphen option.

* What led to the break through?

The help command was the huge break through as I found both cp and ls through it.
I was not aware of the commands, so knowing aws s3 as the base command and followed by help, gave me the full documentation. Read the documenation fully and that helped me in getting the flag.

* On the blue side, how can the learning be used to properly defend the important assets? 

Blind Rule, * should not be used in the principal of the IAM policy. It leads to similar situation like how we were able to access and view the contents of the flag by reading files in the S3 bucket, any unauthorized user could easily do the same. To ensure better security, access should always be granularly scoped, limiting permissions to only specific users or roles, and avoiding unrestricted access to sensitive resources. This approach reduces the risk of exposing critical data to unintended parties.
```
