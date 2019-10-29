# Events and EventBridge<a name="ecr-eventbridge"></a>

Amazon EventBridge enables you to automate your AWS services and respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to EventBridge in near real time\. You can write simple rules to indicate which events are of interest to you, and what automated actions to take when an event matches a rule\. The actions that can be automatically triggered include the following:
+ Log groups in CloudWatch Logs
+ Invoking an AWS Lambda function
+ Invoking Amazon EC2 Run Command
+ Relaying the event to Amazon Kinesis Data Streams
+ Activating an AWS Step Functions state machine
+ Notifying an Amazon SNS topic or an AWS SMS queue

For more information, see [Getting Started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*\.

## Sample Events from Amazon ECR<a name="ecr-eventbridge-bus"></a>

The following are example events from Amazon ECR\.

**Event for a Completed Image Scan**

The following event is sent when each image scan is completed\. Fore more information about Amazon ECR image scanning, see [Image Scanning](image-scanning.md)\.

```
{
    "version": "0",
    "id": "85fc3613-e913-7fc4-a80c-a3753e4aa9ae",
    "detail-type": "ECR Image Scan",
    "source": "aws.ecr",
    "account": "123456789012",
    "time": "2019-10-29T02:36:48Z",
    "region": "us-east-1",
    "resources": [
        "arn:aws:ecr:us-east-1:123456789012:repository/my-repo"
    ],
    "detail": {
        "scan-status": "COMPLETE",
        "repository-name": "my-repo",
        "image-digest": "sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
        "image-tags": []
    }
}
```