# Repository policies<a name="repository-policies"></a>

Amazon ECR uses resource\-based permissions to control access to repositories\. Resource\-based permissions let you specify which IAM users or roles have access to a repository and what actions they can perform on it\. By default, only the repository owner has access to a repository\. You can apply a policy document that allow additional permissions to your repository\.

## Repository policies vs IAM policies<a name="repository-policy-vs-iam-policy"></a>

Amazon ECR repository policies are a subset of IAM policies that are scoped for, and specifically used for, controlling access to individual Amazon ECR repositories\. IAM policies are generally used to apply permissions for the entire Amazon ECR service but can also be used to control access to specific resources as well\.

Both Amazon ECR repository policies and IAM policies are used when determining which actions a specific IAM user or role may perform on a repository\. If a user or role is allowed to perform an action through a repository policy but is denied permission through an IAM policy \(or vice versa\) then the action will be denied\. A user or role only needs to be allowed permission for an action through either a repository policy or an IAM policy but not both for the action to be allowed\.

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

You can use either of these policy types to control access to your repositories, as shown in the following examples\.

This example shows an Amazon ECR repository policy, which allows for a specific IAM user to describe the repository and the images within the repository\.

```
{
  "Version": "2008-10-17",
  "Statement": [{
    "Sid": "ECR Repository Policy",
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::account-id:user/username"
    },
    "Action": [
       "ecr:DescribeImages",
       "ecr:DescribeRepositories"
    ]
  }]
}
```

This example shows an IAM policy that achieves the same goal as above, by scoping the policy to a repository \(specified by the full ARN of the repository\) using the resource parameter\. For more information about Amazon Resource Name \(ARN\) format, see [Resources](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies-resources)\.

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "ECR Repository Policy",
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::account-id:user/username"
    },
    "Action": [
      "ecr:DescribeImages",
      "ecr:DescribeRepositories"
    ],
    "Resource": [
      "arn:aws:ecr:region:account-id:repository/repository-name"
    ]
    }]
}
```

**Topics**
+ [Repository policies vs IAM policies](#repository-policy-vs-iam-policy)
+ [Setting a repository policy statement](set-repository-policy.md)
+ [Deleting a repository policy statement](delete-repository-policy.md)
+ [Repository policy examples](repository-policy-examples.md)