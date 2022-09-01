# Enhanced scanning<a name="image-scanning-enhanced"></a>

Amazon ECR enhanced scanning is an integration with Amazon Inspector which provides vulnerability scanning for your container images\. Your container images are scanned for both operating systems and programing language package vulnerabilities\. You can view the scan findings with both Amazon ECR and with Amazon Inspector directly\. For more information about Amazon Inspector, see [Scanning container images with Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/enable-disable-scanning-ecr.html) in the *Amazon Inspector User Guide*\.

With enhanced scanning, you can choose which repositories are configured for automatic, continuous scanning and which are configured for scan on push\. This is done by setting scan filters\.

## Considerations for enhanced scanning<a name="image-scanning-enhanced-considerations"></a>

The following should be considered when enabling Amazon ECR enhanced scanning\.
+ Enhanced scanning isn't supported in the following Regions:
  + Asia Pacific \(Osaka\) \(`ap-northeast-3`\)
  + Asia Pacific \(Jakarta\) \(`ap-southeast-3`\)
  + Africa \(Cape Town\) \(`af-south-1`\)
+ Amazon Inspector supports scanning for specific operating systems\. For a full list, see [Supported operating systems \- Amazon ECR scanning](https://docs.aws.amazon.com/inspector/latest/user/supported.html#supported-os) in the *Amazon Inspector User Guide*\.
+ Amazon Inspector uses a service\-linked IAM role, which provides the permissions needed to provide enhanced scanning for your repositories\. The service\-linked IAM role is created automatically by Amazon Inspector when enhanced scanning is enabled for your private registry\. For more information, see [Using service\-linked roles for Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/using-service-linked-roles.html) in the *Amazon Inspector User Guide*\.
+ When your private registry has enhanced scanning enabled, all repositories matching the scan filters are scanned using enhanced scanning only\. Any repositories that don't match a filter will have an `Off` scan frequency and won't be scanned\. Manual scans using enhanced scanning aren't supported\. For more information, see [Using filters](image-scanning.md#image-scanning-filters)\.
+ If you specify separate filters for scan on push and continuous scanning where multiple filters match the same repository, then Amazon ECR enforces the continuous scanning filter over the scan on push filter for that repository\.
+ When continuous scanning is enabled for a repository, if an image hasn't been updated in the past 30 days based on the image push timestamp, then continuous scanning is suspended for that image\. Images with suspended scanning will show a scan status of `SCAN_ELIGIBILITY_EXPIRED`\.
+ When enhanced scanning is enabled, Amazon ECR sends an event to EventBridge when the scan frequency for a repository is changed\. Amazon Inspector emits events to EventBridge when an initial scan is completed and when an image scan finding is created, updated, or closed\.

## Required IAM permissions<a name="image-scanning-enhanced-iam"></a>

Amazon ECR enhanced scanning requires an Amazon Inspector service\-linked IAM role and that the IAM principal enabling and using enhanced scanning has permissions to call the Amazon Inspector APIs needed for scanning\. The Amazon Inspector service\-linked IAM role is created automatically by Amazon Inspector when enhanced scanning is enabled for your private registry\. For more information, see [Using service\-linked roles for Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/using-service-linked-roles.html) in the *Amazon Inspector User Guide*\.

The following IAM policy grants the required permissions for enabling and using enhanced scanning\. It includes the permission needed for Amazon Inspector to create the service\-linked IAM role as well as the Amazon Inspector API permissions needed to enable and disable enhanced scanning and retrieve the scan findings\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "inspector2:Enable",
                "inspector2:Disable",
                "inspector2:ListFindings",
                "inspector2:ListAccountPermissions",
                "inspector2:ListCoverage"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": [
                        "inspector2.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

## Enabling enhanced scanning<a name="image-scanning-enhanced-enabling"></a>

### To enable enhanced scanning \(AWS Management Console\)<a name="image-scanning-enhanced-enabling-console"></a>

**To enable enhanced scanning for your private registry \(AWS Management Console\)**

The scanning configuration is defined at the private registry level on a per\-Region basis\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to set the scanning configuration for\.

1. In the navigation pane, choose **Private registry**, **Scanning**\.

1. On the **Scanning configuration** page, for **Scan type** choose **Enhanced scanning**\.

1. \(Optional\) By default, when **Enhanced scanning** is selected, all of your repositories are set for **Continuous scanning**\. You can change the default scanning configuration by deselecting the **Continuously scan all repositories** box\. You can then configure all repositories for scan on push or you can specify separate scan filters for continuous and scan on push\. When scan filters are set, you can choose **Preview repository matches** to verify which repositories in your registry match the defined filters\.
**Important**  
Filters with no wildcard will match all repository names that contain the filter\. Filters with wildcards \(`*`\) match on a repository name where the wildcard replaces zero or more characters in the repository name\.

1. Choose **Save**\.

1. Repeat these steps in each Region in which you want to enable enhanced scanning\.

### To enable enhanced scanning \(AWS CLI\)<a name="image-scanning-enhanced-enabling-cli"></a>

Use the following AWS CLI command to enable enhanced scanning for your private registry using the AWS CLI\. You can specify scan filters using the `rules` object\.
+ [put\-registry\-scanning\-configuration](https://docs.aws.amazon.com/cli/latest/reference/ecr/put-registry-scanning-configuration.html) \(AWS CLI\)

  The following example enables enhanced scanning for your private registry\. By default, when no `rules` are specified, Amazon ECR sets the scanning configuration to continuous scanning for all repositories\.

  ```
  aws ecr put-registry-scanning-configuration \
       --scan-type ENHANCED \
       --region us-east-2
  ```

  The following example enables enhanced scanning for your private registry and specifies a scan filter\. The scan filter in the example enables continuous scanning for all repositories with `prod` in its name\.

  ```
  aws ecr put-registry-scanning-configuration \
       --scan-type ENHANCED \
       --rules '[{"repositoryFilters" : [{"filter":"prod","filterType" : "WILDCARD"}],"scanFrequency" : "CONTINUOUS_SCAN"}]' \
       --region us-east-2
  ```

  The following example enables enhanced scanning for your private registry and specifies multiple scan filters\. The scan filters in the example enables continuous scanning for all repositories with `prod` in its name and enables scan on push only for all other repositories\.

  ```
  aws ecr put-registry-scanning-configuration \
       --scan-type ENHANCED \
       --rules '[{"repositoryFilters" : [{"filter":"prod","filterType" : "WILDCARD"}],"scanFrequency" : "CONTINUOUS_SCAN"},{"repositoryFilters" : [{"filter":"*","filterType" : "WILDCARD"}],"scanFrequency" : "SCAN_ON_PUSH"}]' \
       --region us-west-2
  ```

## Changing the enhanced scanning duration<a name="image-scanning-enhanced-duration"></a>

Amazon Inspector supports configuring the duration that your private repositories are continuously monitored for\. By default, when enhanced scanning is enabled for your Amazon ECR private registry, the Amazon Inspector service continually monitors your repositories until either the image is deleted or enhanced scanning is disabled\. The duration that Amazon Inspector scans your images can be changed using the Amazon Inspector settings\. The available scan durations are **Lifetime \(default\)**, **180 days**, and **30 days**\. When the scan duration for a repository elapses, the scan status of `SCAN_ELIGIBILITY_EXPIRED` is displayed when listing your scan vulnerabilities\. For more information, see [Changing the Amazon ECR automated re\-scan duration](https://docs.aws.amazon.com/inspector/latest/user/enable-disable-scanning-ecr.html#scan-duration-setting) in the *Amazon Inspector User Guide*\.

**To change the enhanced scanning duration setting**

1. Open the Amazon Inspector console at [https://console\.aws\.amazon\.com/inspector/v2/home](https://console.aws.amazon.com/inspector/v2/home)\.

1. In the left navigation, expand **Settings** and then choose **General**\.

1. On the **Settings** page, under **ECR re\-scan duration** choose a setting, then choose **Save**\.

## EventBridge events<a name="image-scanning-enhanced-events"></a>

When enhanced scanning is enabled, Amazon ECR sends an event to EventBridge when the scan frequency for a repository is changed\. Amazon Inspector emits events to EventBridge when an initial scan is completed and when an image scan finding is created, updated, or closed\.

**Event for a repository scan frequency change**

When enhanced scanning is enabled for your registry, the following event is sent by Amazon ECR when there is a change with a resource that has enhanced scanning enabled\. This includes new repositories being created, the scan frequency for a repository being changed, or when images are created or deleted in repositories with enhanced scanning enabled\. For more information, see [Image scanning](image-scanning.md)\.

```
{
	"version": "0",
	"id": "0c18352a-a4d4-6853-ef53-0abEXAMPLE",
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

**Event for an initial image scan \(enhanced scanning\)**

When enhanced scanning is enabled for your registry, the following event is sent by Amazon Inspector when the initial image scan is completed\. The `finding-severity-counts` parameter will only return a value for a severity level if one exists\. For example, if the image contains no findings at `CRITICAL` level, then no critical count is returned\. For more information, see [Enhanced scanning](#image-scanning-enhanced)\.

Event pattern:

```
{
  "source": ["aws.inspector2"],
  "detail-type": ["Inspector2 Scan"]
}
```

Example output:

```
{
    "version": "0",
    "id": "739c0d3c-4f02-85c7-5a88-94a9EXAMPLE",
    "detail-type": "Inspector2 Scan",
    "source": "aws.inspector2",
    "account": "123456789012",
    "time": "2021-12-03T18:03:16Z",
    "region": "us-east-2",
    "resources": [
        "arn:aws:ecr:us-east-2:123456789012:repository/amazon/amazon-ecs-sample"
    ],
    "detail": {
        "scan-status": "INITIAL_SCAN_COMPLETE",
        "repository-name": "arn:aws:ecr:us-east-2:123456789012:repository/amazon/amazon-ecs-sample",
        "finding-severity-counts": {
            "CRITICAL": 7,
            "HIGH": 61,
            "MEDIUM": 62,
            "TOTAL": 158
        },
        "image-digest": "sha256:36c7b282abd0186e01419f2e58743e1bf635808231049bbc9d77e5EXAMPLE",
        "image-tags": [
            "latest"
        ]
    }
}
```

**Event for an image scan finding update \(enhanced scanning\)**

When enhanced scanning is enabled for your registry, the following event is sent by Amazon Inspector when the image scan finding is created, updated, or closed\. For more information, see [Enhanced scanning](#image-scanning-enhanced)\.

Event pattern:

```
{
  "source": ["aws.inspector2"],
  "detail-type": ["Inspector2 Finding"]
}
```

Example output:

```
{
    "version": "0",
    "id": "42dbea55-45ad-b2b4-87a8-afaEXAMPLE",
    "detail-type": "Inspector2 Finding",
    "source": "aws.inspector2",
    "account": "123456789012",
    "time": "2021-12-03T18:02:30Z",
    "region": "us-east-2",
    "resources": [
        "arn:aws:ecr:us-east-2:123456789012:repository/amazon/amazon-ecs-sample/sha256:36c7b282abd0186e01419f2e58743e1bf635808231049bbc9d77eEXAMPLE"
    ],
    "detail": {
        "awsAccountId": "123456789012",
        "description": "In libssh2 v1.9.0 and earlier versions, the SSH_MSG_DISCONNECT logic in packet.c has an integer overflow in a bounds check, enabling an attacker to specify an arbitrary (out-of-bounds) offset for a subsequent memory read. A crafted SSH server may be able to disclose sensitive information or cause a denial of service condition on the client system when a user connects to the server.",
        "findingArn": "arn:aws:inspector2:us-east-2:123456789012:finding/be674aaddd0f75ac632055EXAMPLE",
        "firstObservedAt": "Dec 3, 2021, 6:02:30 PM",
        "inspectorScore": 6.5,
        "inspectorScoreDetails": {
            "adjustedCvss": {
                "adjustments": [],
                "cvssSource": "REDHAT_CVE",
                "score": 6.5,
                "scoreSource": "REDHAT_CVE",
                "scoringVector": "CVSS:3.0/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:N/A:N",
                "version": "3.0"
            }
        },
        "lastObservedAt": "Dec 3, 2021, 6:02:30 PM",
        "packageVulnerabilityDetails": {
            "cvss": [
                {
                    "baseScore": 6.5,
                    "scoringVector": "CVSS:3.0/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:N/A:N",
                    "source": "REDHAT_CVE",
                    "version": "3.0"
                },
                {
                    "baseScore": 5.8,
                    "scoringVector": "AV:N/AC:M/Au:N/C:P/I:N/A:P",
                    "source": "NVD",
                    "version": "2.0"
                },
                {
                    "baseScore": 8.1,
                    "scoringVector": "CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:N/A:H",
                    "source": "NVD",
                    "version": "3.1"
                }
            ],
            "referenceUrls": [
                "https://access.redhat.com/errata/RHSA-2020:3915"
            ],
            "source": "REDHAT_CVE",
            "sourceUrl": "https://access.redhat.com/security/cve/CVE-2019-17498",
            "vendorCreatedAt": "Oct 16, 2019, 12:00:00 AM",
            "vendorSeverity": "Moderate",
            "vulnerabilityId": "CVE-2019-17498",
            "vulnerablePackages": [
                {
                    "arch": "X86_64",
                    "epoch": 0,
                    "name": "libssh2",
                    "packageManager": "OS",
                    "release": "12.amzn2.2",
                    "sourceLayerHash": "sha256:72d97abdfae3b3c933ff41e39779cc72853d7bd9dc1e4800c5294dEXAMPLE",
                    "version": "1.4.3"
                }
            ]
        },
        "remediation": {
            "recommendation": {
                "text": "Update all packages in the vulnerable packages section to their latest versions."
            }
        },
        "resources": [
            {
                "details": {
                    "awsEcrContainerImage": {
                        "architecture": "amd64",
                        "imageHash": "sha256:36c7b282abd0186e01419f2e58743e1bf635808231049bbc9d77e5EXAMPLE",
                        "imageTags": [
                            "latest"
                        ],
                        "platform": "AMAZON_LINUX_2",
                        "pushedAt": "Dec 3, 2021, 6:02:13 PM",
                        "registry": "123456789012",
                        "repositoryName": "amazon/amazon-ecs-sample"
                    }
                },
                "id": "arn:aws:ecr:us-east-2:123456789012:repository/amazon/amazon-ecs-sample/sha256:36c7b282abd0186e01419f2e58743e1bf635808231049bbc9d77EXAMPLE",
                "partition": "N/A",
                "region": "N/A",
                "type": "AWS_ECR_CONTAINER_IMAGE"
            }
        ],
        "severity": "MEDIUM",
        "status": "ACTIVE",
        "title": "CVE-2019-17498 - libssh2",
        "type": "PACKAGE_VULNERABILITY",
        "updatedAt": "Dec 3, 2021, 6:02:30 PM"
    }
}
```

## Retrieving image scan findings<a name="image-scanning-enhanced-describe-scan-findings"></a>

You can retrieve the scan findings for the last completed image scan\. The findings list by severity the software vulnerabilities that were discovered, based on the Common Vulnerabilities and Exposures \(CVEs\) database\.

For troubleshooting details for some common issues when scanning images, see [Troubleshooting image scanning issues](image-scanning-troubleshooting.md)\.

### To retrieve image scan findings \(AWS Management Console\)<a name="image-scanning-enhanced-describe-scan-findings-console"></a>

Use the following steps to retrieve image scan findings using the AWS Management Console\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region where your repository exists\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository that contains the image to retrieve the scan findings for\.

1. On the **Images** page, under the **Vulnerabilities** column, select **See findings** for the image to retrieve the scan findings for\.

1. When viewing the **Findings**, the vulnerability name in the **Name** column is a link to the Amazon Inspector console where you can view more details\.

### To retrieve image scan findings \(AWS CLI\)<a name="image-scanning-enhanced-describe-scan-findings-cli"></a>

Use the following AWS CLI command to retrieve image scan findings using the AWS CLI\. You can specify an image using the `imageTag` or `imageDigest`, both of which can be obtained using the [list\-images](https://docs.aws.amazon.com/cli/latest/reference/ecr/list-images.html) CLI command\.
+ [describe\-image\-scan\-findings](https://docs.aws.amazon.com/cli/latest/reference/ecr/describe-image-scan-findings.html) \(AWS CLI\)

  The following example uses an image tag\.

  ```
  aws ecr describe-image-scan-findings \
       --repository-name name \
       --image-id imageTag=tag_name \
       --region us-east-2
  ```

  The following example uses an image digest\.

  ```
  aws ecr describe-image-scan-findings \
       --repository-name name \
       --image-id imageDigest=sha256_hash \
       --region us-east-2
  ```