# Email-on-EC2-state-change
Send SNS email when the state of an EC2 instance changes (use eventbridge to check the status)


Steps to create this project

Step1: create an EC2 eg. MyEC2
Step2: Create an EventBridge's Target eg. MyTarget

Created an evenbridge event : "event1"

event pattern for stopped is as below:

{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["stopped"],
    "instance-id": ["i-01d85f173790f7b11"]
  }
}

event pattern for running is as below:
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["running"],
    "instance-id": ["i-01d85f173790f7b11"]
  }
}



Step3: create a SNS topic eg. MyTopic with your email id as Subscription

Event's Target is SNS topic: "EC2-stop" with Subscriptions with email id as "sonikashristi@gmail.com"

Access policy as below:

{
  "Version": "2008-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__default_statement_ID",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "SNS:GetTopicAttributes",
        "SNS:SetTopicAttributes",
        "SNS:AddPermission",
        "SNS:RemovePermission",
        "SNS:DeleteTopic",
        "SNS:Subscribe",
        "SNS:ListSubscriptionsByTopic",
        "SNS:Publish"
      ],
      "Resource": "arn:aws:sns:ap-south-1:826563064912:EC2-stop",
      "Condition": {
        "StringEquals": {
          "AWS:SourceOwner": "826563064912"
        }
      }
    },
    {
      "Sid": "AWSEvents_event1_Id5fa69ac9-c0cf-4c32-aa1f-41fa4680a632",
      "Effect": "Allow",
      "Principal": {
        "Service": "events.amazonaws.com"
      },
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:ap-south-1:826563064912:EC2-stop"
    }
  ]
}

