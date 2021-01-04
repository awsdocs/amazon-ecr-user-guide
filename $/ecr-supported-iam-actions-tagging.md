# Using Tag\-Based Access Control<a name="ecr-supported-iam-actions-tagging"></a>

The Amazon ECR CreateRepository API action enables you to specify tags when you create the repository\. For more information, see [Tagging an Amazon ECR repository](ecr-using-tags.md)\.

To enable users to tag repositories on creation, they must have permissions to use the action that creates the resource \(for example, `ecr:CreateRepository`\)\. If tags are specified in the resource\-creating action, Amazon performs additional authorization on the `ecr:CreateRepository` action to verify if users have permissions to create tags\.

You can used tag\-based access control through IAM policies\. The following are examples\.

The following policy would only allow an IAM user to create or tag a repository as `key=environment,value=dev`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCreateTaggedRepository",
            "Effect": "Allow",
            "Action": [
                "ecr:CreateRepository"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/environment": "dev"
                }
            }
        },
        {
            "Sid": "AllowTagRepository",
            "Effect": "Allow",
            "Action": [
                "ecr:TagResource"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/environment": "dev"
                }
            }
        }
    ]
}
```

The following policy would allow an IAM user access to all repositories unless they were tagged as `key=environment,value=prod`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ecr:*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": "ecr:*",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ecr:ResourceTag/environment": "prod"
                }
            }
        }
    ]
}
```