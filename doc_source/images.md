# Images<a name="images"></a>

Amazon ECR stores Docker images in image repositories\. You can use the Docker CLI to push and pull images from your repositories\.

**Important**  
Amazon ECR users require permissions to call `ecr:GetAuthorizationToken` before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.


+ [Pushing an Image](docker-push-ecr-image.md)
+ [Retagging an Image with the AWS CLI](retag-aws-cli.md)
+ [Retagging an Image with the AWS Tools for Windows PowerShell](retag-powershell.md)
+ [Pulling an Image](docker-pull-ecr-image.md)
+ [Container Image Manifest Formats](image-manifest-formats.md)
+ [Using Amazon ECR Images with Amazon ECS](ECR_on_ECS.md)
+ [Deleting an Image](delete_image.md)
+ [Amazon Linux Container Image](amazon_linux_container_image.md)
+ [Amazon ECR Lifecycle Policies](LifecyclePolicies.md)