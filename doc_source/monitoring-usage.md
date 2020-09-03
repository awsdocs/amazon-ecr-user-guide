# Amazon ECR usage metrics<a name="monitoring-usage"></a>

You can use CloudWatch usage metrics to provide visibility into your account's usage of resources\. Use these metrics to visualize your current service usage on CloudWatch graphs and dashboards\.

Amazon ECR usage metrics correspond to AWS service quotas\. You can configure alarms that alert you when your usage approaches a service quota\. For more information about Amazon ECR service quotas, see [Amazon ECR service quotas](service-quotas.md)\.

Amazon ECR publishes the following metrics in the `AWS/Usage` namespace\.


|  Metric  |  Description  | 
| --- | --- | 
|  `CallCount`  |  The number of API action calls from your account\. The resources are defined by the dimensions associated with the metric\. The most useful statistic for this metric is `SUM`, which represents the sum of the values from all contributors during the period defined\.  | 

The following dimensions are used to refine the usage metrics that are published by Amazon ECR\.


|  Dimension  |  Description  | 
| --- | --- | 
|  `Service`  |  The name of the AWS service containing the resource\. For Amazon ECR usage metrics, the value for this dimension is `ECR`\.  | 
|  `Type`  |  The type of entity that is being reported\. Currently, the only valid value for Amazon ECR usage metrics is `API`\.  | 
|  `Resource`  |  The type of resource that is running\. Currently, Amazon ECR returns information on your API usage for the following API actions\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECR/latest/userguide/monitoring-usage.html)  | 
|  Class  |  The class of resource being tracked\. Currently, Amazon ECR does not use the class dimension\.  | 