# Troubleshooting image scanning issues<a name="image-scanning-troubleshooting"></a>

The following are common image scan failures\. You can view errors like this in the Amazon ECR console by displaying the image details or through the API or AWS CLI by using the `DescribeImageScanFindings` API\.

UnsupportedImageError  
You may get an `UnsupportedImageError` error when attempting to perform a basic scan on an image that was built using an operating system that Amazon ECR doesn't support basic image scanning for\. Amazon ECR supports package vulnerability scanning for major versions of Amazon Linux, Amazon Linux 2, Debian, Ubuntu, CentOS, Oracle Linux, Alpine, and RHEL Linux distributions\. Once a distribution loses support from its vendor, Amazon ECR may no longer support scanning it for vulnerabilities\. Amazon ECR does not support scanning images built from the [Docker scratch](https://hub.docker.com/_/scratch) image\.  
When using enhanced scanning, Amazon Inspector supports scanning for specific operating systems and media types\. For a full list, see [Supported operating systems and media types](https://docs.aws.amazon.com/inspector/latest/user/enable-disable-scanning-ecr.html#ecr-supported-media) in the *Amazon Inspector User Guide*\.

An `UNDEFINED` severity level is returned  
You may receive a scan finding that has a severity level of `UNDEFINED`\. The following are the common causes for this:  
+ The vulnerability was not assigned a priority by the CVE source\.
+ The vulnerability was assigned a priority that Amazon ECR did not recognize\.
To determine the severity and description of a vulnerability, you can view the CVE directly from the source\.

## Understanding scan status `SCAN_ELIGIBILITY_EXPIRED`<a name="image-scanning-troubleshooting-eligibility"></a>

When enhanced scanning using Amazon Inspector is enabled for your private registry and you are viewing your scan vulnerabilities, you may see a scan status of `SCAN_ELIGIBILITY_EXPIRED`\. The following are the most common causes of this\.
+ If an image hasn't been updated in the past 30 days, based on the image push timestamp, then continuous scanning is suspended for that image\. Images with suspended scanning will show a scan status of `SCAN_ELIGIBILITY_EXPIRED`\.
+ If the **ECR re\-scan duration** is changed in the Amazon Inspector console and that time elapses, the scan status of the image is changed to `inactive` with a reason code of `expired`, and all associated findings for the image are scheduled to be closed\. This results in the Amazon ECR console listing the scan status as `SCAN_ELIGIBILITY_EXPIRED`\.