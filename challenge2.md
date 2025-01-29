# Challenge 2

## Challenge Statement
**Google Analytics** - We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong? Join our queue and get the secret flag.

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?

The IAM policy allows me to both send and receive messages from the SQS queue "wiz-tbic-analytics-sqs-queue-ca7a1b2". This means I can interact with the queue and receive messages, which is a crucial step in extracting the flag.

* What don't I have access to?

There's no permission granted for other actions like deleting or modifying messages in the queue. 
Additionally, there is no access to resources outside of the specified queue (like other SQS queues or AWS services).

* What do I find interesting?

The policy allows a broad principal ("Principal": "*") to interact with the queue, meaning anyone can send or receive messages. This could potentially be a security risk, but it also allows for easy access to the queue, which is critical for solving the challenge.

* etc
```

## Solution
* To solve the challenge, I used the AWS CLI to receive messages from the queue. The specific command I ran was:
```
aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --attribute-names All
```
* This command returned a message with a body containing a URL. The URL in the message's body was like:
```json
{
    "URL": "https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html",
}
```

* I opened the URL provided in the body of the message. After doing so, I was able to retrieve the flag, which was hidden within the content.

## Reflection
```
* What was your approach?

My approach was simple—start by reviewing the IAM policy and then use the AWS CLI to interact with the SQS queue. I expected that the message in the queue would contain a clue or a direct link to the flag. Once I received the message, the URL provided led me directly to the flag.

* What was the biggest challenge?

The biggest challenge was understanding how to interact with the SQS queue and how to use the AWS CLI effectively. While the steps seemed straightforward, having the right permissions and knowing the correct commands were essential to solving the challenge.

* How did you overcome the challenges?

I overcame the challenges by carefully reading the IAM policy and testing the AWS CLI commands to retrieve messages from the queue. Once I got a response, the URL was clear and led me directly to the solution.

* What led to the break through?

The breakthrough came when I realized that the message contained a URL that pointed to an S3 bucket. From there, it was just a matter of accessing the URL to retrieve the flag.

* On the blue side, how can the learning be used to properly defend the important assets? 

This challenge highlights the importance of securing access to critical resources like SQS queues. While this challenge allowed open access to the queue, in real-world scenarios, we would want to restrict access to the queue using more precise IAM policies. We should avoid broad permissions (e.g., "Principal": "*") and instead limit access to trusted identities. Additionally, securing URLs and making sure that sensitive information is not exposed in plain text is crucial in protecting against unauthorized access.
```
