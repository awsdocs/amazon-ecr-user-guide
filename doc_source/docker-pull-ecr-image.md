# Pulling an image<a name="docker-pull-ecr-image"></a>

If you would like to run a Docker image that is available in Amazon ECR, you can pull it to your local environment with the docker pull command\. You can do this from either your default registry or from a registry associated with another AWS account\. To use an Amazon ECR image in an Amazon ECS task definition, see [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md)\.

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

**To pull a Docker image from an Amazon ECR repository**

1. Authenticate your Docker client to the Amazon ECR registry that you intend to pull your image from\. Authentication tokens must be obtained for each registry used, and the tokens are valid for 12 hours\. For more information, see [Registry authentication](Registries.md#registry_auth)\.

1. \(Optional\) Identify the image to pull\.
   + You can list the repositories in a registry with the aws ecr describe\-repositories command:

     ```
     aws ecr describe-repositories
     ```

     The example registry above has a repository called `amazonlinux`\.
   + You can describe the images within a repository with the aws ecr describe\-images command:

     ```
     aws ecr describe-images --repository-name amazonlinux
     ```

     The example repository above has an image tagged as `latest` and `2016.09`, with the image digest `sha256:f1d4ae3f7261a72e98c6ebefe9985cf10a0ea5bd762585a43e0700ed99863807`\.

1. Pull the image using the docker pull command\. The image name format should be `registry/repository[:tag]` to pull by tag, or `registry/repository[@digest]` to pull by digest\.

   ```
   docker pull aws_account_id.dkr.ecr.us-west-2.amazonaws.com/amazonlinux:latest
   ```
**Important**  
If you receive a `repository-url not found: does not exist or no pull access` error, you may need to authenticate your Docker client with Amazon ECR\. For more information, see [Registry authentication](Registries.md#registry_auth)\.