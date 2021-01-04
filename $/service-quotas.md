# Amazon ECR service quotas<a name="service-quotas"></a>

The following table provides the default service quotas for Amazon Elastic Container Registry \(Amazon ECR\)\.


****  

| Service quota | Description | Default quota value | 
| --- | --- | --- | 
|  Registered repositories  |  The maximum number of repositories that you can create per Region\.  |  10,000  | 
|  Image per repository  |  The maximum number of images per repository\.  |  10,000  | 

The following table provides the default rate quotas for each of the Amazon ECR API actions involved with the image push and image pull actions\.


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECR/latest/userguide/service-quotas.html)

The following table provides other quotas for Amazon ECR and Docker images that cannot be changed\.

**Note**  
The layer part information mentioned in the following table is only applicable if you are calling the Amazon ECR API actions directly to initiate multipart uploads for image push operations\. This is a rare action\. We recommend that you use the Docker CLI to pull, tag, and push images\.


| Service quota | Description | Quota value | 
| --- | --- | --- | 
|  Layer parts  |  The maximum number of layer parts\. This is only applicable if you are using Amazon ECR API actions directly to initiate multipart uploads for image push operations\.  |  1,000  | 
|  Maximum layer size  |  The maximum size \(MiB\) of a layer\. \*\*  |  10,000  | 
|  Minimum layer part size  |  The minimum size \(MiB\) of a layer part\. This is only applicable if you are using Amazon ECR API actions directly to initiate multipart uploads for image push operations\.  |  5  | 
|  Maximum layer part size  |  The maximum size \(MiB\) of a layer part\. This is only applicable if you are using Amazon ECR API actions directly to initiate multipart uploads for image push operations\.  |  10  | 
|  Tags per image  |  The maximum number of tags per image\.  |  1000  | 
|  Lifecycle policy length  |  The maximum number of characters in a lifecycle policy\.  |  30,720  | 
|  Rules per lifecycle policy  |  The maximum number of rules in a lifecycle policy\.  |  50  | 
|  Rate of image scans  |  The maximum number of image scans per image, per day\.  |  1  | 

\*\* The maximum layer size listed here is calculated by multiplying the maximum layer part size \(10 MiB\) by the maximum number of layer parts \(1,000\)\.

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