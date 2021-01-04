# Amazon ECR monitoring<a name="monitoring"></a>

You can monitor your Amazon ECR API usage with Amazon CloudWatch, which collects and processes raw data from Amazon ECR into readable, near real\-time metrics\. These statistics are recorded for a period of two weeks, so that you can access historical information and gain perspective on your API usage\. Amazon ECR metric data is automatically sent to CloudWatch in one\-minute periods\. For more information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

Amazon ECR provides metrics based on your API usage for authorization, image push, and image pull actions\.

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon ECR and your AWS solutions\. We recommend that you collect monitoring data from the resources that make up your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. Before you start monitoring Amazon ECR, however, you should create a monitoring plan that includes answers to the following questions:
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

The next step is to establish a baseline for normal Amazon ECR performance in your environment by measuring performance at various times and under different load conditions\. As you monitor Amazon ECR, store historical monitoring data so that you can compare it with new performance data, identify normal performance patterns and performance anomalies, and devise methods to address issues\.

**Topics**
+ [Visualizing your service quotas and setting alarms](monitoring-quotas-alarms.md)
+ [Amazon ECR usage metrics](monitoring-usage.md)
+ [Amazon ECR usage reports](usage-reports.md)
+ [Amazon ECR events and EventBridge](ecr-eventbridge.md)
+ [Logging Amazon ECR actions with AWS CloudTrail](logging-using-cloudtrail.md)