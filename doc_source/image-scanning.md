# Image Scanning<a name="image-scanning"></a>

Amazon ECR image scanning helps in identifying software vulnerabilities in your container images\. Amazon ECR uses the Common Vulnerabilities and Exposures \(CVEs\) database from the open source CoreOS Clair project and provides you with a list of scan findings\. You can review the scan findings for information about the security of the container images that are being deployed\. For more information about CoreOS Clair, see [CoreOS Clair](https://github.com/coreos/clair)\.

You can manually scan container images stored in Amazon ECR, or you can configure your repositories to scan images when you push them to a repository\. The last completed image scan findings can be retrieved for each image\. Amazon ECR sends an event to Amazon EventBridge \(formerly called CloudWatch Events\) when an image scan is completed\. For more information, see [Events and EventBridge](ecr-eventbridge.md)\.

**Topics**
+ [Configuring a Repository to Scan on Push](#scanning-repository)
+ [Manually Scanning an Image](#manual-scan)
+ [Retrieving Scan Findings](#describe-scan-findings)
+ [Troubleshooting](#image-scanning-troubleshooting)

## Configuring a Repository to Scan on Push<a name="scanning-repository"></a>

You can configure the image scan settings either for a new repository during creation or for an existing repository\. When **scan on push** is enabled, images are scanned after being pushed to a repository\. If **scan on push** is disabled on a repository then you must manually start each image scan to get the scan results\.

**Topics**
+ [Creating a New Repository to Scan on Push](#scanning-new-repository)
+ [Configure an Existing Repository to Scan on Push](#scanning-existing-repository)

### Creating a New Repository to Scan on Push<a name="scanning-new-repository"></a>

When a new repository is configured to **scan on push**, all new images pushed to the repository will be scanned\. Results from the last completed image scan can then be retrieved\. For more information, see [Retrieving Scan Findings](#describe-scan-findings)\.

For AWS Management Console steps, see [Creating a Repository](repository-create.md)\.

#### To create a repository configured for scan on push \(AWS CLI\)<a name="scanning-repo-cli"></a>

Use the following command to create a new repository with image **scan on push** configured\.
+ [create\-repository](https://docs.aws.amazon.com/cli/latest/reference/ecr/create-repository.html) \(AWS CLI\)

  ```
  aws ecr create-repository --repository-name name --image-scanning-configuration scanOnPush=true --region us-east-2
  ```

#### To create a repository configured for scan on push \(AWS Tools for Windows PowerShell\)<a name="scanning-repo-powershell"></a>

Use the following command to create a new repository with image **scan on push** configured\.
+ [New\-ECRRepository](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECRRepository.html) \(AWS Tools for Windows PowerShell\)

  ```
  New-ECRRepository -RepositoryName name -ImageScanningConfiguration scanOnPush=true -Region us-east-2 -Force
  ```

### Configure an Existing Repository to Scan on Push<a name="scanning-existing-repository"></a>

Your existing repositories can be configured to scan images when you push them to a repository\. This setting will apply to future image pushes\. Results from the last completed image scan can then be retrieved\. For more information, see [Retrieving Scan Findings](#describe-scan-findings)\.

For AWS Management Console steps, see [Editing a Repository](repository-edit.md)\.

#### To edit the settings of an existing repository \(AWS CLI\)<a name="scanning-existing-repo-cli"></a>

Use the following command to edit the image scanning settings of an existing repository\.
+ [put\-image\-scanning\-configuration](https://docs.aws.amazon.com/cli/latest/reference/ecr/put-image-scanning-configuration.html) \(AWS CLI\)

  ```
  aws ecr put-image-scanning-configuration --repository-name name --image-scanning-configuration scanOnPush=true --region us-east-2
  ```
**Note**  
To disable image **scan on push** for a repository, specify `scanOnPush=false`\.

#### To edit the settings of an existing repository \(AWS Tools for Windows PowerShell\)<a name="scanning-existing-repo-powershell"></a>

Use the following command to edit the image scanning settings of an existing repository\.
+ [New\-ECRRepository](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECRRepository.html) \(AWS Tools for Windows PowerShell\)

  ```
  New-ECRRepository -RepositoryName name -ImageScanningConfiguration scanOnPush=true -Region us-east-2 -Force
  ```

## Manually Scanning an Image<a name="manual-scan"></a>

You can start image scans manually when you want to scan images in repositories that are not configured to **scan on push**\. An image can only be scanned once per day\. This limit includes the initial **scan on push**, if enabled, and any manual scans\.

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

Use the following AWS Tools for Windows PowerShell command to start a manual scan of an image\. You can specify an image using the `imageTag` or `imageDigest`, both of which can be obtained using the [list\-images](https://docs.aws.amazon.com/cli/latest/reference/ecr/list-images.html) CLI command\.
+ [New\-ECRRepository](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECRRepository.html) \(AWS Tools for Windows PowerShell\)

  ```
  New-ECRRepository -RepositoryName name -ImageScanningConfiguration scanOnPush=true -Region us-east-2 -Force
  ```

## Retrieving Scan Findings<a name="describe-scan-findings"></a>

You can retrieve the scan findings for the last completed image scan\. The findings list by severity the software vulnerabilities that were discovered, based on the Common Vulnerabilities and Exposures \(CVEs\) database\.

### To retrieve image scan findings \(console\)<a name="describe-scan-findings-console"></a>

Use the following steps to retrieve image scan findings using the AWS Management Console\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository that contains the image to scan\.

1. On the **Images** page, under the **Vulnerabilities** column, select **Details** for the image to retrieve the scan findings for\.

### To retrieve image scan findings \(AWS CLI\)<a name="describe-scan-findings-cli"></a>

Use the following AWS CLI command to start a manual scan of an image\. You can specify an image using the `imageTag` or `imageDigest`, both of which can be obtained using the [list\-images](https://docs.aws.amazon.com/cli/latest/reference/ecr/list-images.html) CLI command\.
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

Use the following AWS Tools for Windows PowerShell command to start a manual scan of an image\. You can specify an image using the `imageTag` or `imageDigest`, both of which can be obtained using the [list\-images](https://docs.aws.amazon.com/cli/latest/reference/ecr/list-images.html) CLI command\.
+ [New\-ECRRepository](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECRRepository.html) \(AWS Tools for Windows PowerShell\)

  ```
  New-ECRRepository -RepositoryName name -ImageScanningConfiguration scanOnPush=true -Region us-east-2 -Force
  ```

## Troubleshooting<a name="image-scanning-troubleshooting"></a>

The following are common image scan failures\. You can view errors like this in the Amazon ECR console by displaying the image details or through the API or AWS CLI by using the `DescribeImageScanFindings` API\.

UnsupportedImageError: Operating system ID and version could not be detected  
You may get a `UnsupportedImageError` error when attempting to scan an image that was built using an unsupported operating system\. Amazon ECR supports package vulnerability scanning for major versions of Amazon Linux, Amazon Linux 2, Debian, Ubuntu, CentOS, Oracle Linux, Alpine, and RHEL Linux distributions\. Amazon ECR does not support scanning images built from the [Docker scratch](https://hub.docker.com/_/scratch) image\.