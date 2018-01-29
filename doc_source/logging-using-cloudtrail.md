# Logging Amazon ECR API Calls By Using AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon ECR is integrated with CloudTrail, a service that captures all of the API calls made by or on behalf of Amazon ECR in your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the Amazon ECR console or from the Amazon ECR API\. Using the information collected by CloudTrail, you can determine what request was made to Amazon ECR, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [http://docs.aws.amazon.com/awscloudtrail/latest/userguide/](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon ECR Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to Amazon ECR actions are tracked in log files\. Amazon ECR records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All of the Amazon ECR actions are logged by CloudTrail and are documented in the [Amazon Elastic Container Registry API Reference](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/)\. For example, calls to the  **GetAuthorizationToken**, **CreateRepository** and **SetRepositoryPolicy** operations generate entries in the CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the **userIdentity** field in the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted by using Amazon S3 server\-side encryption \(SSE\)\.

You can choose to have CloudTrail publish Amazon SNS notifications when new log files are delivered if you want to take quick action upon log file delivery\. For more information, see [Configuring Amazon SNS Notifications](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Amazon ECR log files from multiple AWS regions and multiple AWS accounts into a single S3 bucket\. For more information, see [Aggregating CloudTrail Log Files to a Single Amazon S3 Bucket](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding Amazon ECR Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public API calls\.