# Private repository policy examples<a name="repository-policy-examples"></a>

The following examples show policy statements that you could use to control the permissions that authenticated users have to Amazon ECR repositories\.

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

## Example: Allow one or more IAM users<a name="IAM_within_account"></a>

The following repository policy allows one or more IAM users to push and pull images to and from a repository\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowPushPull",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::account-id:user/push-pull-user-1",
                    "arn:aws:iam::account-id:user/push-pull-user-2"
                ]
            },
            "Action": [
                "ecr:BatchGetImage",
                "ecr:BatchCheckLayerAvailability",
                "ecr:CompleteLayerUpload",
                "ecr:GetDownloadUrlForLayer",
                "ecr:InitiateLayerUpload",
                "ecr:PutImage",
                "ecr:UploadLayerPart"
            ]
        }
    ]
}
```

## Example: Allow another account<a name="IAM_allow_other_accounts"></a>

The following repository policy allows a specific account to push images\.

**Important**  
The account you are granting permissions to must have the Region you are creating the repository policy in enabled, otherwise an error will occur\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCrossAccountPush",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::account-id:root"
            },
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:CompleteLayerUpload",
                "ecr:InitiateLayerUpload",
                "ecr:PutImage",
                "ecr:UploadLayerPart"
            ]
        }
    ]
}
```

The following repository policy allows some IAM users to pull images \(*pull\-user\-1* and *pull\-user\-2*\) while providing full access to another \(*admin\-user*\)\.

**Note**  
For more complicated repository policies that are not currently supported in the AWS Management Console, you can apply the policy with the [https://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html](https://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html) AWS CLI command\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowPull",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::account-id:user/pull-user-1",
                    "arn:aws:iam::account-id:user/pull-user-2"
                ]
            },
            "Action": [
                "ecr:BatchGetImage",
                "ecr:GetDownloadUrlForLayer"
            ]
        },
        {
            "Sid": "AllowAll",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::account-id:user/admin-user"
            },
            "Action": [
                "ecr:*"
            ]
        }
    ]
}
```

## Example: Deny all<a name="IAM_deny_all"></a>

The following repository policy denies all users in all accounts the ability to pull images\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyPull",
            "Effect": "Deny",
            "Principal": "*",
            "Action": [
                "ecr:BatchGetImage",
                "ecr:GetDownloadUrlForLayer"
            ]
        }
    ]
}
```

## Example: Restricting access to specific IP addresses<a name="IAM_restrict_ip"></a>

The following example denies permissions to any user to perform any Amazon ECR operations when applied to a repository from a specific range of addresses\.

The condition in this statement identifies the `54.240.143.*` range of allowed Internet Protocol version 4 \(IPv4\) IP addresses\.

The `Condition` block uses the `NotIpAddress` conditions and the `aws:SourceIp` condition key, which is an AWS\-wide condition key\. For more information about these condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\. The `aws:sourceIp` IPv4 values use the standard CIDR notation\. For more information, see [IP Address Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_IPAddress) in the *IAM User Guide*\.

```
{
    "Version": "2012-10-17",
    "Id": "ECRPolicyId1",
    "Statement": [
        {
            "Sid": "IPAllow",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "ecr:*",
            "Condition": {
                "NotIpAddress": {
                    "aws:SourceIp": "54.240.143.0/24"
                }
            }
        }
    ]
}
```

## Example: Allow an AWS service<a name="IAM_service_linked"></a>

The following repository policy allows AWS CodeBuild access to the Amazon ECR API actions necessary for integration with that service\. When using the following example, you should use the `aws:SourceArn` and `aws:SourceAccount` condition keys to scope which resources can assume these permissions\. For more information, see [Amazon ECR sample for CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-ecr.html) in the *AWS CodeBuild User Guide*\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"CodeBuildAccess",
         "Effect":"Allow",
         "Principal":{
            "Service":"codebuild.amazonaws.com"
         },
         "Action":[
            "ecr:BatchGetImage",
            "ecr:GetDownloadUrlForLayer"
         ],
         "Condition":{
            "ArnLike":{
               "aws:SourceArn":"arn:aws:codebuild:region:123456789012:project/project-name"
            },
            "StringEquals":{
               "aws:SourceAccount":"123456789012"
            }
         }
      }
   ]
}
```