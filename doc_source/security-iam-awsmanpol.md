# AWS managed policies for Amazon Elastic Container Registry<a name="security-iam-awsmanpol"></a>

Amazon ECR provides several managed policies that you can attach to IAM users or Amazon EC2 instances\. These policies allow differing levels of control over access to Amazon ECR resources and API operations\. You can apply these policies directly or use them as starting points for creating your own policies\. For more information about each API operation mentioned in these policies, see [Actions](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.

**Topics**
+ [`AmazonEC2ContainerRegistryFullAccess`](#security-iam-awsmanpol-AmazonEC2ContainerRegistryFullAccess)
+ [`AmazonEC2ContainerRegistryPowerUser`](#security-iam-awsmanpol-AmazonEC2ContainerRegistryPowerUser)
+ [`AmazonEC2ContainerRegistryReadOnly`](#security-iam-awsmanpol-AmazonEC2ContainerRegistryReadOnly)
+ [`AWSECRPullThroughCache_ServiceRolePolicy`](#security-iam-awsmanpol-AWSECRPullThroughCache_ServiceRolePolicy)
+ [`ECRReplicationServiceRolePolicy`](#security-iam-awsmanpol-ECRReplicationServiceRolePolicy)
+ [Amazon ECR updates to AWS managed policies](#security-iam-awsmanpol-updates)

## `AmazonEC2ContainerRegistryFullAccess`<a name="security-iam-awsmanpol-AmazonEC2ContainerRegistryFullAccess"></a>

You can attach the `AmazonEC2ContainerRegistryFullAccess` policy to your IAM identities\.

You can use this managed policy as a starting point to create your own IAM policy based on your specific requirements\. For example, you can create a policy specifically for providing an IAM user or role with full administrator access to manage the use of Amazon ECR\. With the [Amazon ECR Lifecycle Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/LifecyclePolicies.html) feature, you can specify the lifecycle management of images in a repository\. Lifecycle policy events are reported as CloudTrail events\. Amazon ECR is integrated with AWS CloudTrail so it can display your lifecycle policy events directly in the Amazon ECR console\. The `AmazonEC2ContainerRegistryFullAccess` managed IAM policy includes the `cloudtrail:LookupEvents` permission to facilitate this behavior\.

**Permissions details**

This policy includes the following permissions:
+ `ecr` – Allows principals full access to all Amazon ECR APIs\.
+ `cloudtrail` – Allows principals to looks up management events or AWS CloudTrail Insights events that are captured by CloudTrail\.

The `AmazonEC2ContainerRegistryFullAccess` policy is as follows\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:*",
                "cloudtrail:LookupEvents"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateServiceLinkedRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": [
                        "replication.ecr.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

## `AmazonEC2ContainerRegistryPowerUser`<a name="security-iam-awsmanpol-AmazonEC2ContainerRegistryPowerUser"></a>

You can attach the `AmazonEC2ContainerRegistryPowerUser` policy to your IAM identities\.

This policy grants administrative permissions that allow IAM users to read and write to repositories, but doesn't allow them to delete repositories or change the policy documents that are applied to them\.

**Permissions details**

This policy includes the following permissions:
+ `ecr` – Allows principals to read and write to respositores, as well as read lifecycle policies\. Principals aren't granted permission to delete repositories or change the lifecycle policies that are applied to them\.

The `AmazonEC2ContainerRegistryPowerUser` policy is as follows\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:GetLifecyclePolicy",
                "ecr:GetLifecyclePolicyPreview",
                "ecr:ListTagsForResource",
                "ecr:DescribeImageScanFindings",
                "ecr:InitiateLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:CompleteLayerUpload",
                "ecr:PutImage"
            ],
            "Resource": "*"
        }
    ]
}
```

## `AmazonEC2ContainerRegistryReadOnly`<a name="security-iam-awsmanpol-AmazonEC2ContainerRegistryReadOnly"></a>

You can attach the `AmazonEC2ContainerRegistryReadOnly` policy to your IAM identities\.

This policy grants read\-only permissions to Amazon ECR\. This includes the ability to list repositories and images within the repositories\. It also includes the ability to pull images from Amazon ECR with the Docker CLI\.

**Permissions details**

This policy includes the following permissions:
+ `ecr` – Allows principals to read repositories and their respective lifecycle policies\.

The `AmazonEC2ContainerRegistryReadOnly` policy is as follows\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:GetLifecyclePolicy",
                "ecr:GetLifecyclePolicyPreview",
                "ecr:ListTagsForResource",
                "ecr:DescribeImageScanFindings"
            ],
            "Resource": "*"
        }
    ]
}
```

## `AWSECRPullThroughCache_ServiceRolePolicy`<a name="security-iam-awsmanpol-AWSECRPullThroughCache_ServiceRolePolicy"></a>

You can't attach the `AWSECRPullThroughCache_ServiceRolePolicy` managed IAM policy to your IAM entities\. This policy is attached to a service\-linked role that allows Amazon ECR to push images to your repositories through the pull through cache workflow\. For more information, see [Using service\-linked roles for Amazon ECR](using-service-linked-roles.md)\.

## `ECRReplicationServiceRolePolicy`<a name="security-iam-awsmanpol-ECRReplicationServiceRolePolicy"></a>

You can't attach the `ECRReplicationServiceRolePolicy` managed IAM policy to your IAM entities\. This policy is attached to a service\-linked role that allows Amazon ECR to perform actions on your behalf\. For more information, see [Using service\-linked roles for Amazon ECR](using-service-linked-roles.md)\.

## Amazon ECR updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for Amazon ECR since the time that this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon ECR Document history page\.

 


| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSECRPullThroughCache\_ServiceRolePolicy](#security-iam-awsmanpol-AWSECRPullThroughCache_ServiceRolePolicy) – New policy  |  Amazon ECR added a new policy\. This policy is associated with the `AWSServiceRoleForECRPullThroughCache` service\-linked role for the pull through cache feature\.  | November 29, 2021 | 
|  [ECRReplicationServiceRolePolicy](#security-iam-awsmanpol-ECRReplicationServiceRolePolicy) – New policy  |  Amazon ECR added a new policy\. This policy is associated with the `AWSServiceRoleForECRReplication` service\-linked role for the replication feature\.  | December 4, 2020 | 
|  [AmazonEC2ContainerRegistryFullAccess](#security-iam-awsmanpol-AmazonEC2ContainerRegistryFullAccess) – Update to an existing policy  |  Amazon ECR added new permissions to the `AmazonEC2ContainerRegistryFullAccess` policy\. These permissions allow principals to create the Amazon ECR service\-linked role\.  | December 4, 2020 | 
|  [AmazonEC2ContainerRegistryReadOnly](#security-iam-awsmanpol-AmazonEC2ContainerRegistryReadOnly) – Update to an existing policy  |  Amazon ECR added new permissions to the `AmazonEC2ContainerRegistryReadOnly` policy which allow principals to read lifecycle policies, list tags, and describe the scan findings for images\.  | December 10, 2019 | 
|  [AmazonEC2ContainerRegistryPowerUser](#security-iam-awsmanpol-AmazonEC2ContainerRegistryPowerUser) – Update to an existing policy  |  Amazon ECR added new permissions to the `AmazonEC2ContainerRegistryPowerUser` policy\. They allow principals to read lifecycle policies, list tags, and describe the scan findings for images\.  | December 10, 2019 | 
|  [AmazonEC2ContainerRegistryFullAccess](#security-iam-awsmanpol-AmazonEC2ContainerRegistryFullAccess) – Update to an existing policy  |  Amazon ECR added new permissions to the `AmazonEC2ContainerRegistryFullAccess` policy\. They allow principals to look up management events or AWS CloudTrail Insights events that are captured by CloudTrail\.  | November 10, 2017 | 
|  [AmazonEC2ContainerRegistryReadOnly](#security-iam-awsmanpol-AmazonEC2ContainerRegistryReadOnly) – Update to an existing policy  |  Amazon ECR added new permissions to the `AmazonEC2ContainerRegistryReadOnly` policy\. They allow principals to describe Amazon ECR images\.  | October 11, 2016 | 
|  [AmazonEC2ContainerRegistryPowerUser](#security-iam-awsmanpol-AmazonEC2ContainerRegistryPowerUser) – Update to an existing policy  |  Amazon ECR added new permissions to the `AmazonEC2ContainerRegistryPowerUser` policy\. They allow principals to describe Amazon ECR images\.  | October 11, 2016 | 
|  [AmazonEC2ContainerRegistryReadOnly](#security-iam-awsmanpol-AmazonEC2ContainerRegistryReadOnly) – New policy  |  Amazon ECR added a new policy which grants grants read\-only permissions to Amazon ECR\. These permissions include the ability to list repositories and images within the repositories\. They also include the ability to pull images from Amazon ECR with the Docker CLI\.  | December 21, 2015 | 
|  [AmazonEC2ContainerRegistryPowerUser](#security-iam-awsmanpol-AmazonEC2ContainerRegistryPowerUser) – New policy  |  Amazon ECR added a new policy which grants administrative permissions that allow IAM users to read and write to repositories but doesn't allow them to delete repositories or change the policy documents that are applied to them\.  | December 21, 2015 | 
|  [AmazonEC2ContainerRegistryFullAccess](#security-iam-awsmanpol-AmazonEC2ContainerRegistryFullAccess) – New policy  |  Amazon ECR added a new policy\. This policy grants full access to Amazon ECR\.  | December 21, 2015 | 
|  Amazon ECR started tracking changes  |  Amazon ECR started tracking changes for AWS managed policies\.  | June 24, 2021 | 