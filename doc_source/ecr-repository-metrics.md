# Amazon ECR repository metrics<a name="ecr-repository-metrics"></a>

Amazon ECR sends repository pull count metrics to Amazon CloudWatch\. Amazon ECR metric data is automatically sent to CloudWatch in 1\-minute periods\. For more information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**Topics**
+ [Enabling CloudWatch metrics](#enable_cloudwatch)
+ [Available metrics and dimensions](#available_cloudwatch_metrics)
+ [Viewing Amazon ECR metrics](#ecr-repository-metrics-viewing)

## Enabling CloudWatch metrics<a name="enable_cloudwatch"></a>

Amazon ECR sends repository metrics automatically for all repositories\. There is no need to take any manual steps\.

## Available metrics and dimensions<a name="available_cloudwatch_metrics"></a>

The following sections list the metrics and dimensions that Amazon ECR sends to Amazon CloudWatch\.

### Amazon ECR metrics<a name="ecr-metrics"></a>

Amazon ECR provides metrics for you to monitor your repositories\. You can measure the pull count\.

The `AWS/ECR` namespace includes the following metrics\.

`RepositoryPullCount`  
The total number of pulls for the images in the repository\.  
Valid dimensions: `RepositoryName`\.  
Valid statistics: Average, Minimum, Maximum, Sum, Sample Count\. The most useful statistic is Sum\.  
Unit: Integer\.

### Dimensions for Amazon ECR metrics<a name="ecs-metrics-dimensions"></a>

Amazon ECR metrics use the `AWS/ECR` namespace and provide metrics for the following dimensions\.

`RepositoryName`  
This dimension filters the data that you request for all container images in a specified repository\.

## Viewing Amazon ECR metrics<a name="ecr-repository-metrics-viewing"></a>

You can view Amazon ECR repository metrics on the CloudWatch console\. The CloudWatch console provides a fine\-grained and customizable display of your resources\.

### Viewing Amazon ECR metrics using the CloudWatch console<a name="viewing_metrics_console"></a>

Amazon ECR repository metrics can be viewed on the CloudWatch console\. The console provides the most detailed view of Amazon ECR metrics, and you can tailor the views to suit your needs\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**To view metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation, choose **Metrics**, **All metrics**\. 

1. On the **Browse** tab, under **AWS Namespaces**, choose **ECR**\.

1. Choose the metrics to view\. Repository metrics are scoped as **ECR > Repository Metrics**\.