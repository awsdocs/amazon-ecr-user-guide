# Basic scanning<a name="image-scanning-basic"></a>

Amazon ECR provides basic scanning type which uses the Common Vulnerabilities and Exposures \(CVEs\) database from the open\-source Clair project\. With basic scanning enabled on your private registry, you can configure repository filters to specify which repositories are set to scan on push or you can perform manual scans\. Amazon ECR provides a list of scan findings\. Each container image may be scanned once per 24 hours\. Amazon ECR uses the Common Vulnerabilities and Exposures \(CVEs\) database from the open\-source Clair project and provides a list of scan findings\. You can review the scan findings for information about the security of the container images that are being deployed\. For more information about Clair, see [Clair](https://github.com/quay/clair) on GitHub\.

Amazon ECR uses the severity for a CVE from the upstream distribution source if available, otherwise we use the Common Vulnerability Scoring System \(CVSS\) score\. The CVSS score can be used to obtain the NVD vulnerability severity rating\. For more information, see [NVD Vulnerability Severity Ratings](https://nvd.nist.gov/vuln-metrics/cvss)\. 

When basic scanning is used, you may specify scan on push filters to specify which repositories are set to do an image scan when new images are pushed\. Any repositories not matching a scan on push filter will be set to the **manual** scan frequency which means to perform a scan, you must manually trigger the scan\. The last completed image scan findings can be retrieved for each image\. Amazon ECR sends an event to Amazon EventBridge \(formerly called CloudWatch Events\) when an image scan is completed\. For more information, see [Amazon ECR events and EventBridge](ecr-eventbridge.md)\.

For troubleshooting details for some common issues when scanning images, see [Troubleshooting image scanning issues](image-scanning-troubleshooting.md)\.

## Enabling basic scanning<a name="image-scanning-basic-enabling"></a>

By default, Amazon ECR enables basic scanning on all private registries\. As a result, unless you've changed the scanning settings on your private registry there should be no need to enable basic scanning\. You may use the following steps to verify that basic scanning is enabled and define one or more scan on push filters\.

**To enable basic scanning for your private registry \(AWS Management Console\)**

The scanning configuration is defined at the private registry level on a per\-Region basis\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to set the scanning configuration for\.

1. In the navigation pane, choose **Private registry**, **Scanning**\.

1. On the **Scanning configuration** page, For **Scan type** choose **Basic scanning**\.

1. By default all of your repositories are set for **Manual** scanning\. You can optionally enable scan on push by specifying **Scan on push filters**\. You can set scan on push for all repositories or individual repositories\. For more information, see [Using filters](image-scanning.md#image-scanning-filters)\.

## Manually scanning an image<a name="manual-scan"></a>

You can start image scans manually when you want to scan images in repositories that aren't configured to **scan on push**\. An image can only be scanned once each day\. This limit includes the initial **scan on push**, if enabled, and any manual scans\.

For troubleshooting details for some common issues when scanning images, see [Troubleshooting image scanning issues](image-scanning-troubleshooting.md)\.

### To start a manual scan of an image \(console\)<a name="manual-scan-console"></a>

Use the following steps to start a manual image scan using the AWS Management Console\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository that contains the image to scan\.

1. On the **Images** page, select the image to scan and then choose **Scan**\.

### To start a manual scan of an image \(AWS CLI\)<a name="manual-scan-cli"></a>

Use the following AWS CLI command to start a manual scan of an image\. You can specify an image using the `imageTag` or `imageDigest`, both of which can be obtained using the [list\-images](https://docs.aws.amazon.com/cli/latest/reference/ecr/list-images.html) CLI command\.
+ [start\-image\-scan](https://docs.aws.amazon.com/cli/latest/reference/ecr/start-image-scan.html) \(AWS CLI\)

  The following example uses an image tag\.

  ```
  aws ecr start-image-scan --repository-name name --image-id imageTag=tag_name --region us-east-2
  ```

  The following example uses an image digest\.

  ```
  aws ecr start-image-scan --repository-name name --image-id imageDigest=sha256_hash --region us-east-2
  ```

### To start a manual scan of an image \(AWS Tools for Windows PowerShell\)<a name="manual-scan-powershell"></a>

Use the following AWS Tools for Windows PowerShell command to start a manual scan of an image\. You can specify an image using the `ImageId_ImageTag` or `ImageId_ImageDigest`, both of which can be obtained using the [Get\-ECRImage](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRImage.html) CLI command\.
+ [Get\-ECRImageScanFinding](https://docs.aws.amazon.com/powershell/latest/reference/items/Start-ECRImageScan.html) \(AWS Tools for Windows PowerShell\)

  The following example uses an image tag\.

  ```
  Start-ECRImageScan -RepositoryName name -ImageId_ImageTag tag_name -Region us-east-2 -Force
  ```

  The following example uses an image digest\.

  ```
  Start-ECRImageScan -RepositoryName name -ImageId_ImageDigest sha256_hash -Region us-east-2 -Force
  ```

## Retrieving image scan findings<a name="describe-scan-findings"></a>

You can retrieve the scan findings for the last completed image scan\. The findings list by severity the software vulnerabilities that were discovered, based on the Common Vulnerabilities and Exposures \(CVEs\) database\.

For troubleshooting details for some common issues when scanning images, see [Troubleshooting image scanning issues](image-scanning-troubleshooting.md)\.

### To retrieve image scan findings \(console\)<a name="describe-scan-findings-console"></a>

Use the following steps to retrieve image scan findings using the AWS Management Console\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository that contains the image to retrieve the scan findings for\.

1. On the **Images** page, under the **Vulnerabilities** column, select **Details** for the image to retrieve the scan findings for\.

### To retrieve image scan findings \(AWS CLI\)<a name="describe-scan-findings-cli"></a>

Use the following AWS CLI command to retrieve image scan findings using the AWS CLI\. You can specify an image using the `imageTag` or `imageDigest`, both of which can be obtained using the [list\-images](https://docs.aws.amazon.com/cli/latest/reference/ecr/list-images.html) CLI command\.
+ [describe\-image\-scan\-findings](https://docs.aws.amazon.com/cli/latest/reference/ecr/describe-image-scan-findings.html) \(AWS CLI\)

  The following example uses an image tag\.

  ```
  aws ecr describe-image-scan-findings --repository-name name --image-id imageTag=tag_name --region us-east-2
  ```

  The following example uses an image digest\.

  ```
  aws ecr describe-image-scan-findings --repository-name name --image-id imageDigest=sha256_hash --region us-east-2
  ```

### To retrieve image scan findings \(AWS Tools for Windows PowerShell\)<a name="describe-scan-findings-powershell"></a>

Use the following AWS Tools for Windows PowerShell command to retrieve image scan findings\. You can specify an image using the `ImageId_ImageTag` or `ImageId_ImageDigest`, both of which can be obtained using the [Get\-ECRImage](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRImage.html) CLI command\.
+ [Get\-ECRImageScanFinding](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRImageScanFinding.html) \(AWS Tools for Windows PowerShell\)

  The following example uses an image tag\.

  ```
  Get-ECRImageScanFinding -RepositoryName name -ImageId_ImageTag tag_name -Region us-east-2
  ```

  The following example uses an image digest\.

  ```
  Get-ECRImageScanFinding -RepositoryName name -ImageId_ImageDigest sha256_hash -Region us-east-2
  ```