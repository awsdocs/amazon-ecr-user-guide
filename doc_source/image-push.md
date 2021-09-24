# Pushing an image<a name="image-push"></a>

You can push your Docker images, manifest lists, and Open Container Initiative \(OCI\) images and compatible artifacts to your private repositories\. The following pages describe these in more detail\.

Amazon ECR also provides a way to replicate your images to other repositories, across Regions in your own registry and across different accounts, by specifying a replication configuration in your private registry settings\. For more information, see [Private registry settings](registry-settings.md)\.

**Topics**
+ [Required IAM permissions for pushing an image](#image-push-iam)
+ [Pushing a Docker image](docker-push-ecr-image.md)
+ [Pushing a multi\-architecture image](docker-push-multi-architecture-image.md)
+ [Pushing a Helm chart](push-oci-artifact.md)

## Required IAM permissions for pushing an image<a name="image-push-iam"></a>

Amazon ECR requires that users have the following permissions to push images\. Following the best practice of granting least privilege, you can scope these permissions down to a specific repository or you can grant the permissions for all repositories\. A user must authenticate to each Amazon ECR registry they want to push images to by requesting an authorization token\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

The following IAM policy grants the required permissions for pushing an image without scoping to a specific repository\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:CompleteLayerUpload",
                "ecr:GetAuthorizationToken",
                "ecr:UploadLayerPart",
                "ecr:InitiateLayerUpload",
                "ecr:BatchCheckLayerAvailability",
                "ecr:PutImage"
            ],
            "Resource": "*"
        }
    ]
}
```

The following IAM policy grants the required permissions for pushing an image and scopes to a specific repository\. The repository must be specified as a full Amazon Resource Name \(ARN\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:CompleteLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:InitiateLayerUpload",
                "ecr:BatchCheckLayerAvailability",
                "ecr:PutImage"
            ],
            "Resource": "arn:aws:ecr:region:111122223333:repository/repository-name"
        },
        {
            "Effect": "Allow",
            "Action": "ecr:GetAuthorizationToken",
            "Resource": "*"
        }
    ]
}
```