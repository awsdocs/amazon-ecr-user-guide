# Amazon Linux container image<a name="amazon_linux_container_image"></a>

The Amazon Linux container image is built from the same software components that are included in the Amazon Linux AMI\. It's available for use in any environment as a base image for Docker workloads\. If you're using the Amazon Linux AMI for applications in Amazon EC2, you can containerize your applications with the Amazon Linux container image\.

You can use the Amazon Linux container image in your local development environment and then push your application to AWS using Amazon ECS\. For more information, see [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md)\.

The Amazon Linux container image is available on Amazon ECR Public and on [Docker Hub](https://hub.docker.com/_/amazonlinux/)\. Support for the Amazon Linux container image can be found by visiting the [AWS developer forums](https://forums.aws.amazon.com/forum.jspa?forumID=228)\.

**To pull the Amazon Linux container image from Amazon ECR Public**

1. Authenticate your Docker client to the Amazon Linux Public registry\. Authentication tokens are valid for 12 hours\. For more information, see [Private registry authentication](registry_auth.md)\.
**Note**  
The ecr\-public commands are available in the AWS CLI starting with version `1.18.1.187`, however we recommend using the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws
   ```

   The output is as follows:

   ```
   Login succeeded
   ```

1. Pull the Amazon Linux container image using the docker pull command\. To view the Amazon Linux container image on the Amazon ECR Public Gallery, see [Amazon ECR Public Gallery \- amazonlinux](https://gallery.ecr.aws/amazonlinux/amazonlinux)\.

   ```
   docker pull public.ecr.aws/amazonlinux/amazonlinux:latest
   ```

1. \(Optional\) Run the container locally\.

   ```
   docker run -it public.ecr.aws/amazonlinux/amazonlinux /bin/bash
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