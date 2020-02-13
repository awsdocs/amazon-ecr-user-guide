# Images<a name="images"></a>

Amazon Elastic Container Registry \(Amazon ECR\) stores Docker and Open Container Initiative \(OCI\) images in image repositories\. You can use the Docker CLI to push and pull images from your repositories\.

**Important**  
Amazon ECR requires that users have allow permissions to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.