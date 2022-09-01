# Amazon ECR service quotas<a name="service-quotas"></a>

The following table provides the default service quotas for Amazon Elastic Container Registry \(Amazon ECR\)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECR/latest/userguide/service-quotas.html)

## Managing your Amazon ECR service quotas in the AWS Management Console<a name="service-quotas-console"></a>

Amazon ECR has integrated with Service Quotas, an AWS service that enables you to view and manage your quotas from a central location\. For more information, see [What Is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) in the *Service Quotas User Guide*\.

Service Quotas makes it easy to look up the value of all Amazon ECR service quotas\.

**To view Amazon ECR service quotas \(AWS Management Console\)**

1. Open the Service Quotas console at [https://console\.aws\.amazon\.com/servicequotas/](https://console.aws.amazon.com/servicequotas/)\.

1. In the navigation pane, choose **AWS services**\.

1. From the **AWS services** list, search for and select **Amazon Elastic Container Registry \(Amazon ECR\)**\.

   In the **Service quotas** list, you can see the service quota name, applied value \(if it is available\), AWS default quota, and whether the quota value is adjustable\.

1. To view additional information about a service quota, such as the description, choose the quota name\.

To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-increase.html) in the *Service Quotas User Guide*\.

### Creating a CloudWatch alarm to monitor API usage metrics<a name="service-quota-alarm"></a>

Amazon ECR provides CloudWatch usage metrics that correspond to the AWS service quotas for each of the APIs involved with the registry authentication, image push, and image pull actions\. In the Service Quotas console, you can visualize your usage on a graph and configure alarms that alert you when your usage approaches a service quota\. For more information, see [Amazon ECR usage metrics](monitoring-usage.md)\.

Use the following steps to create a CloudWatch alarm based on one of the Amazon ECR API usage metrics\.

**To create an alarm based on your Amazon ECR usage quotas \(AWS Management Console\)**

1. Open the Service Quotas console at [https://console\.aws\.amazon\.com/servicequotas/](https://console.aws.amazon.com/servicequotas/)\.

1. In the navigation pane, choose **AWS services**\.

1. From the **AWS services** list, search for and select **Amazon Elastic Container Registry \(Amazon ECR\)**\.

1. In the **Service quotas** list, select the Amazon ECR usage quota you want to create an alarm for\.

1. In the Amazon CloudWatch Events alarms section, choose **Create**\.

1. For **Alarm threshold**, choose the percentage of your applied quota value that you want to set as the alarm value\.

1. For **Alarm name**, enter a name for the alarm and then choose **Create**\.