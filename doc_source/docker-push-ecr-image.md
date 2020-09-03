# Pushing an image<a name="docker-push-ecr-image"></a>

You can push your Docker images to an Amazon ECR repository with the docker push command\.

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

Amazon ECR also supports creating and pushing Docker manifest lists which are used for multi\-architecture images\. Each image referenced in a manifest list must already be pushed to your repository\. For more information, see [Pushing a multi\-architecture image](docker-push-multi-architecture-image.md)\.

**To push a Docker image to an Amazon ECR repository**

1. Authenticate your Docker client to the Amazon ECR registry to which you intend to push your image\. Authentication tokens must be obtained for each registry used, and the tokens are valid for 12 hours\. For more information, see [Registry authentication](Registries.md#registry_auth)\.

1. If your image repository does not exist in the registry you intend to push to yet, create it\. For more information, see [Creating a repository](repository-create.md)\.

1. Identify the image to push\. Run the docker images command to list the images on your system\.

   ```
   docker images
   ```

   You can identify an image with the *repository:tag* value or the image ID in the resulting command output\.

1. <a name="image-tag-step"></a>Tag your image with the Amazon ECR registry, repository, and optional image tag name combination to use\. The registry format is `aws_account_id.dkr.ecr.region.amazonaws.com`\. The repository name should match the repository that you created for your image\. If you omit the image tag, we assume that the tag is `latest`\.

   The following example tags an image with the ID *e9ae3c220b23* as `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app`

   ```
   docker tag e9ae3c220b23 aws_account_id.dkr.ecr.region.amazonaws.com/my-web-app
   ```

1. <a name="image-push-step"></a>Push the image using the docker push command:

   ```
   docker push aws_account_id.dkr.ecr.region.amazonaws.com/my-web-app
   ```

1. \(Optional\) Apply any additional tags to your image and push those tags to Amazon ECR by repeating [Step 4](#image-tag-step) and [Step 5](#image-push-step)\. You can apply up to 100 tags per image in Amazon ECR\.