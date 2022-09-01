# Amazon ECR events and EventBridge<a name="ecr-eventbridge"></a>

Amazon EventBridge enables you to automate your AWS services and to respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to EventBridge in near real time\. You can write simple rules to indicate which events are of interest to you and include automated actions to take when an event matches a rule\. The actions that can be automatically triggered include the following:
+ Adding events to log groups in CloudWatch Logs
+ Invoking an AWS Lambda function
+ Invoking Amazon EC2 Run Command
+ Relaying the event to Amazon Kinesis Data Streams
+ Activating an AWS Step Functions state machine
+ Notifying an Amazon SNS topic or an Amazon SQS queue

For more information, see [Getting Started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*\.

## Sample events from Amazon ECR<a name="ecr-eventbridge-bus"></a>

The following are example events from Amazon ECR\. Events are emitted on a best effort basis\.

**Event for a completed image push**

The following event is sent when each image push is completed\. For more information, see [Pushing a Docker image](docker-push-ecr-image.md)\.

```
{
    "version": "0",
    "id": "13cde686-328b-6117-af20-0e5566167482",
    "detail-type": "ECR Image Action",
    "source": "aws.ecr",
    "account": "123456789012",
    "time": "2019-11-16T01:54:34Z",
    "region": "us-west-2",
    "resources": [],
    "detail": {
        "result": "SUCCESS",
        "repository-name": "my-repository-name",
        "image-digest": "sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
        "action-type": "PUSH",
        "image-tag": "latest"
    }
}
```

**Event for a completed image scan \(basic scanning\)**

When basic scanning is enabled for your registry, the following event is sent when each image scan is completed\. The `finding-severity-counts` parameter will only return a value for a severity level if one exists\. For example, if the image contains no findings at `CRITICAL` level, then no critical count is returned\. For more information, see [Basic scanning](image-scanning-basic.md)\.

**Note**  
For details about events that Amazon Inspector emits when enhanced scanning is enabled, see [EventBridge events](image-scanning-enhanced.md#image-scanning-enhanced-events)\.

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
        "arn:aws:ecr:us-east-1:123456789012:repository/my-repository-name"
    ],
    "detail": {
        "scan-status": "COMPLETE",
        "repository-name": "my-repository-name",
        "finding-severity-counts": {
	       "CRITICAL": 10,
	       "MEDIUM": 9
	     },
        "image-digest": "sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
        "image-tags": []
    }
}
```

**Event for a change notification on a resource with enhanced scanning enabled \(enhanced scanning\)**

When enhanced scanning is enabled for your registry, the following event is sent by Amazon ECR when there is a change with a resource that has enhanced scanning enabled\. This includes new repositories being created, the scan frequency for a repository being changed, or when images are created or deleted in repositories with enhanced scanning enabled\. For more information, see [Image scanning](image-scanning.md)\.

```
{
	"version": "0",
	"id": "0c18352a-a4d4-6853-ef53-0ab8638973bf",
	"detail-type": "ECR Scan Resource Change",
	"source": "aws.ecr",
	"account": "123456789012",
	"time": "2021-10-14T20:53:46Z",
	"region": "us-east-1",
	"resources": [],
	"detail": {
		"action-type": "SCAN_FREQUENCY_CHANGE",
		"repositories": [{
				"repository-name": "repository-1",
				"repository-arn": "arn:aws:ecr:us-east-1:123456789012:repository/repository-1",
				"scan-frequency": "SCAN_ON_PUSH",
				"previous-scan-frequency": "MANUAL"
			},
			{
				"repository-name": "repository-2",
				"repository-arn": "arn:aws:ecr:us-east-1:123456789012:repository/repository-2",
				"scan-frequency": "CONTINUOUS_SCAN",
				"previous-scan-frequency": "SCAN_ON_PUSH"
			},
			{
				"repository-name": "repository-3",
				"repository-arn": "arn:aws:ecr:us-east-1:123456789012:repository/repository-3",
				"scan-frequency": "CONTINUOUS_SCAN",
				"previous-scan-frequency": "SCAN_ON_PUSH"
			}
		],
		"resource-type": "REPOSITORY",
		"scan-type": "ENHANCED"
	}
}
```

**Event for an image deletion**

The following event is sent when an image is deleted\. For more information, see [Deleting an image](delete_image.md)\.

```
{
    "version": "0",
    "id": "dd3b46cb-2c74-f49e-393b-28286b67279d",
    "detail-type": "ECR Image Action",
    "source": "aws.ecr",
    "account": "123456789012",
    "time": "2019-11-16T02:01:05Z",
    "region": "us-west-2",
    "resources": [],
    "detail": {
        "result": "SUCCESS",
        "repository-name": "my-repository-name",
        "image-digest": "sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
        "action-type": "DELETE",
        "image-tag": "latest"
    }
}
```