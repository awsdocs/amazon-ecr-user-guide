# Amazon ECR Repository Policy Examples<a name="RepositoryPolicyExamples"></a>

The following examples show policy statements that you could use to control the permissions that users have to Amazon ECR repositories\.

**Important**  
Amazon ECR requires that users have allow permissions to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.

## Example: Allow All AWS Accounts To Pull Images<a name="IAM_all_accounts"></a>

The following repository policy allows all AWS accounts to pull images\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "AllowPull",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:BatchCheckLayerAvailability"
      ]
    }
  ]
}
```

## Example: Allow Specific IAM Users Within Your Account<a name="IAM_within_account"></a>

The following repository policy allows IAM users within your account to push and pull images\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "AllowPushPull",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/push-pull-user-1",
          "arn:aws:iam::123456789012:user/push-pull-user-2"
        ]
      },
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:BatchCheckLayerAvailability",
        "ecr:PutImage",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload"
      ]
    }
  ]
}
```

## Example: Allow Other Accounts<a name="IAM_allow_other_accounts"></a>

The following repository policy allows a specific account to push images\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "AllowCrossAccountPush",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchCheckLayerAvailability",
        "ecr:PutImage",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload"
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
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "AllowPull",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/pull-user-1",
          "arn:aws:iam::123456789012:user/pull-user-2"
        ]
      },
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:BatchCheckLayerAvailability"
      ]
    },
    {
      "Sid": "AllowAll",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/admin-user"
      },
      "Action": [
        "ecr:*"
      ]
    }
  ]
}
```

## Example: Deny All<a name="IAM_deny_all"></a>

The following repository policy denies all users the ability to pull images\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "DenyPull",
      "Effect": "Deny",
      "Principal": "*",
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:BatchCheckLayerAvailability"
      ]
    }
  ]
}
```

## Example: Service\-Linked Role<a name="IAM_service_linked"></a>

The following repository policy allows AWS CodeBuild access to the Amazon ECR API actions necessary for integration with that service\. For more information, see [Amazon ECR Sample for CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-ecr.html) in the *AWS CodeBuild User Guide*\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CodeBuildAccess",
      "Effect": "Allow",
      "Principal": {
        "Service": "codebuild.amazonaws.com"
      },
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:BatchGetImage",
        "ecr:GetDownloadUrlForLayer"
      ]
    }
  ]
}
```