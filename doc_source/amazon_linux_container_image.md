# Amazon Linux container image<a name="amazon_linux_container_image"></a>

The Amazon Linux container image is built from the same software components that are included in the Amazon Linux AMI\. It is available for use in any environment as a base image for Docker workloads\. If you are already using the Amazon Linux AMI for applications in Amazon EC2, then you can easily containerize your applications with the Amazon Linux container image\.

You can use the Amazon Linux container image in your local development environment and then push your application to the AWS Cloud using Amazon ECS\. For more information, see [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md)\.

The Amazon Linux container image is available in Amazon ECR and on [Docker Hub](https://hub.docker.com/_/amazonlinux/)\. Support for the Amazon Linux container image can be found by visiting the [AWS developer forums](https://forums.aws.amazon.com/forum.jspa?forumID=228)\.

**To pull the Amazon Linux container image from Amazon ECR**

1. Authenticate your Docker client to the Amazon Linux container image Amazon ECR registry\. Authentication tokens are valid for 12 hours\. For more information, see [Registry authentication](Registries.md#registry_auth)\.
**Note**  
The get\-login\-password command is available in the AWS CLI starting with version `1.17.10`\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 137112412989.dkr.ecr.us-east-1.amazonaws.com
   ```

   Output:

   ```
   Login succeeded
   ```
**Important**  
If you receive an error, install or upgrade to the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

1. \(Optional\) You can list the images within the Amazon Linux repository with the aws ecr list\-images command\. The `latest` tag always corresponds with the latest Amazon Linux container image that is available\.

   ```
   aws ecr list-images --region us-east-1 --registry-id 137112412989 --repository-name amazonlinux
   ```

1. Pull the Amazon Linux container image using the docker pull command\.

   ```
   docker pull 137112412989.dkr.ecr.us-east-1.amazonaws.com/amazonlinux:latest
   ```

1. \(Optional\) Run the container locally\.

   ```
   docker run -it 137112412989.dkr.ecr.us-east-1.amazonaws.com/amazonlinux:latest /bin/bash
   ```

**To pull the Amazon Linux container image from Docker Hub**

1. Pull the Amazon Linux container image using the docker pull command\.

   ```
   docker pull amazonlinux
   ```

1. \(Optional\) Run the container locally\.

   ```
   docker run -it amazonlinux:latest /bin/bash
   ```