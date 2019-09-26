# Images<a name="images"></a>

Amazon Elastic Container Registry \(Amazon ECR\) stores Docker images in image repositories\. You can use the Docker CLI to push and pull images from your repositories\.

**Important**  
Amazon ECR requires that users have allow permissions to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.

**Topics**
+ [Pushing an Image](docker-push-ecr-image.md)
+ [Pulling an Image](docker-pull-ecr-image.md)
+ [Deleting an Image](delete_image.md)
+ [Retagging an Image](image-retag.md)
+ [Amazon ECR Lifecycle Policies](LifecyclePolicies.md)
+ [Image Tag Mutability](image-tag-mutability.md)
+ [Container Image Manifest Formats](image-manifest-formats.md)
+ [Using Amazon ECR Images with Amazon ECS](ECR_on_ECS.md)
+ [Using Amazon ECR Images with Amazon EKS](ECR_on_EKS.md)
+ [Amazon Linux Container Image](amazon_linux_container_image.md)