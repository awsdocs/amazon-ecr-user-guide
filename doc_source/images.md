# Images<a name="images"></a>

Amazon Elastic Container Registry \(Amazon ECR\) stores Docker images, Open Container Initiative \(OCI\) images, and OCI compatible artifacts in repositories\. You can use the Docker CLI or your preferred client to push and pull images to and from your repositories\.

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

**Topics**
+ [Pushing an image](docker-push-ecr-image.md)
+ [Pushing a multi\-architecture image](docker-push-multi-architecture-image.md)
+ [Pushing a Helm chart](push-oci-artifact.md)
+ [Pulling an image](docker-pull-ecr-image.md)
+ [Deleting an image](delete_image.md)
+ [Retagging an image](image-retag.md)
+ [Lifecycle policies](LifecyclePolicies.md)
+ [Image tag mutability](image-tag-mutability.md)
+ [Image scanning](image-scanning.md)
+ [Container image manifest formats](image-manifest-formats.md)
+ [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md)
+ [Using Amazon ECR Images with Amazon EKS](ECR_on_EKS.md)
+ [Amazon Linux container image](amazon_linux_container_image.md)