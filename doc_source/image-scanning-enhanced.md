# Enhanced scanning<a name="image-scanning-enhanced"></a>

Amazon ECR enhanced scanning is an integration with Amazon Inspector which performs vulnerability scanning for your container images\. Your container images are scanned for both operating systems and programing language package vulnerabilities\. You can view the scan findings with both Amazon ECR and with Amazon Inspector directly\. For more information about Amazon Inspector, see [Scanning container images with Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/enable-disable-scanning-ecr.html) in the *Amazon Inspector User Guide*\.

With enhanced scanning, you can choose which repositories are configured for automatic, continuous scanning and which are configured for scan on push\. This is done by setting scan filters\.

## Considerations for enhanced scanning<a name="image-scanning-enhanced-considerations"></a>

The following should be considered when enabling Amazon ECR enhanced scanning\.
+ Amazon Inspector uses a service\-linked IAM role, which provides the permissions needed to provide enhanced scanning for your repositories\. The service\-linked IAM role is created automatically by Amazon Inspector when enhanced scanning is enabled for your private registry\. For more information, see [Using service\-linked roles for Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/using-service-linked-roles.html) in the *Amazon Inspector User Guide*\.
+ When your private registry has enhanced scanning enabled, all repositories matching the scan filters are scanned using enhanced scanning only\. Any repositories that don't match a filter will have a `Manual` scan frequency, but won't be scanned\. Manual scans using enhanced scanning isn't supported\.
+ When continuous scanning is enabled for a repository, if an image hasn't been updated in the past 30 days based on the image push timestamp, then continuous scanning is suspended for that image\.
+ When enhanced scanning is enabled, Amazon ECR sends an event to EventBridge when the scan frequency for a repository is changed\. Amazon Inspector emits events to EventBridge when an initial scan is completed and when an image scan finding is created, updated, or closed\.

## Enabling enhanced scanning<a name="image-scanning-enhanced-enabling"></a>

### To enable enhanced scanning \(AWS Management Console\)<a name="image-scanning-enhanced-enabling-console"></a>

**To enable enhanced scanning for your private registry \(AWS Management Console\)**

The scanning configuration is defined at the private registry level on a per\-Region basis\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to set the scanning configuration for\.

1. In the navigation pane, choose **Private registry**, **Scanning**\.

1. On the **Scanning configuration** page, for **Scan type** choose **Enhanced scanning**\.

1. \(Optional\) By default, when **Enhanced scanning** is specified, all of your repositories are set for **Continuous scanning**\. You can optionally configure all of your repositories for scan on push or you can specify separate scan filters for continuous and scan on push\. When scan filters are set, you can choose **Preview repository matches** to verify which repositories in your registry match the defined filters\.

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

## EventBridge events<a name="image-scanning-enhanced-events"></a>

When enhanced scanning is enabled, Amazon ECR sends an event to EventBridge when the scan frequency for a repository is changed\. Amazon Inspector emits events to EventBridge when an initial scan is completed and when an image scan finding is created, updated, or closed\.

**Event for a repository scan frequency change**

When enhanced scanning is enabled for your registry, the following event is sent by Amazon ECR when there is a change with a resource that has enhanced scanning enabled\. This includes new repositories being created, the scan frequency for a repository being changed, or when images are created or deleted in repositories with enhanced scanning enabled\. For more information, see [Image scanning](image-scanning.md)\.

```
{
	"version": "0",
	"id": "0c18352a-a4d4-6853-ef53-0ab8638973bf",
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

```
{
	"version": "0",
	"id": "85fc3613-e913-7fc4-a80c-a3753e4aa9ae",
	"detail-type": "Inspector Scan",
	"source": "aws.inspector",
	"account": "123456789012",
	"time": "2019-10-29T02:36:48Z",
	"region": "us-east-1",
	"resources": [
		"arn:aws:ecr:us-east-1:123456789012:repository/my-repository-name"
	],
	"detail": {
		"scan-status": "INITIAL_SCAN_COMPLETE",
		"repository-name": "my-repository-name",
		"finding-severity-counts": {
			"CRITICAL": 10,
			"HIGH": 2,
			"MEDIUM": 9
		},
		"image-digest": "sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
		"image-tags": []
	}
}
```

**Event for an image scan finding update \(enhanced scanning\)**

When enhanced scanning is enabled for your registry, the following event is sent by Amazon Inspector when the image scan finding is created, updated, or closed\. For more information, see [Enhanced scanning](#image-scanning-enhanced)\.

```
{
	"version": "0",
	"id": "85fc3613-e913-7fc4-a80c-a3753e4aa9ae",
	"detail-type": "Inspector Finding",
	"source": "aws.inspector",
	"account": "123456789012",
	"time": "2019-10-29T02:36:48Z",
	"region": "us-east-1",
	"resources": [
		"arn:aws:ecr:us-east-1:123456789012:repository/my-repository-name",
		"i-12345678"
	],
	"detail": {
		"awsAccountId": "123456789012",
		"description": "Multiple integer overflows in libwebp allows attackers to have unspecified impact via unknown vectors.",
		"findingArn": "arn:aws:inspector:us-east-1:123456789012:finding/610857596309d79db0cb09cEXAMPLE",
		"firstObservedAt": "2021-05-12T19:45:41.343Z",
		"cvss": [{
			"baseScore": 3.3,
			"scoringVector": "CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:L",
			"source": "NVD",
			"version": "3.1"
		}],
		"referenceUrls": [
			"https://bugzilla.redhat.com/show_bug.cgi"
		],
		"relatedVulnerabilities": null,
		"source": "NVD",
		"sourceUrl": "https://nvd.nist.gov/vuln/detail/CVE-2016-9085",
		"vendorCreatedAt": "2017-02-03T15:59:00Z",
		"vendorSeverity": "LOW",
		"vendorUpdatedAt": "2021-02-25T17:15:00Z",
		"vulnerabilityId": "CVE-2016-9085",
		"vulnerablePackages": [{
			"arch": "AMD64",
			"epoch": 0,
			"name": "libwebp-dev",
			"release": "1",
			"sourceLayerHash": "sha256:3c4c73959f29d362478996015b71e5f4ba5360b835ec71c2435ff0EXAMPLE",
			"version": "0.5.2"
		}],
		"recommendationText": "Update all packages in the vulnerable packages section to their latest versions.",
		"recommendationUrl": null,
		"imageArchitecture": "amd64",
		"imageAuthor": null,
		"imageHash": "sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
		"imageTags": [
			"2.0.01"
		],
		"imagePlatform": null,
		"imagePushedAt": "2020-04-09T03:49:22Z",
		"registry": "123456789012",
		"repositoryName": "tscloud/iteportal-ui",
		"resourceId": "123456789012/tscloud/iteportal-ui/sha256:7f5b2640fe6fb4f46592dfd3410c4a79dac4f89e4782432e0378abcd1234",
		"imagePartition": "N/A",
		"imageRegion": "N/A",
		"tags": null,
		"resourceType": "AWS_ECR_CONTAINER_IMAGE",
		"score": 3.3,
		"status": "ACTIVE",
		"title": "CVE-2016-9085 - libwebp-dev",
		"type": "PACKAGE_VULNERABILITY",
		"updatedAt": "2021-06-05T17:30:42.773Z"
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