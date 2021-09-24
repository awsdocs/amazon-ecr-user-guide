# Pushing a Docker image<a name="docker-push-ecr-image"></a>

You can push your container images to an Amazon ECR repository with the docker push command\. Amazon ECR also supports creating and pushing Docker manifest lists, which are used for multi\-architecture images\. Each image referenced in a manifest list must already be pushed to your repository\. For more information, see [Pushing a multi\-architecture image](docker-push-multi-architecture-image.md)\.

**To push a Docker image to an Amazon ECR repository**

The Amazon ECR repository must exist before you push the image\. For more information, see [Creating a private repository](repository-create.md)\.

1. Authenticate your Docker client to the Amazon ECR registry to which you intend to push your image\. Authentication tokens must be obtained for each registry used, and the tokens are valid for 12 hours\. For more information, see [Private registry authentication](registry_auth.md)\.

   To authenticate Docker to an Amazon ECR registry, run the aws ecr get\-login\-password command\. When passing the authentication token to the docker login command, use the value `AWS` for the username and specify the Amazon ECR registry URI you want to authenticate to\. If authenticating to multiple registries, you must repeat the command for each registry\.
**Important**  
If you receive an error, install or upgrade to the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
   ```

1. If your image repository doesn't exist in the registry you intend to push to yet, create it\. For more information, see [Creating a private repository](repository-create.md)\.

1. Identify the local image to push\. Run the docker images command to list the container images on your system\.

   ```
   docker images
   ```

   You can identify an image with the *repository:tag* value or the image ID in the resulting command output\.

1. <a name="image-tag-step"></a>Tag your image with the Amazon ECR registry, repository, and optional image tag name combination to use\. The registry format is `aws_account_id.dkr.ecr.region.amazonaws.com`\. The repository name should match the repository that you created for your image\. If you omit the image tag, we assume that the tag is `latest`\.

   The following example tags a local image with the ID *e9ae3c220b23* as `aws_account_id.dkr.ecr.region.amazonaws.com``/my-repository:tag`\.

   ```
   docker tag e9ae3c220b23 aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
   ```

1. <a name="image-push-step"></a>Push the image using the docker push command:

   ```
   docker push aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
   ```

1. \(Optional\) Apply any additional tags to your image and push those tags to Amazon ECR by repeating [Step 4](#image-tag-step) and [Step 5](#image-push-step)\.