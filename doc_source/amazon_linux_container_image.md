# Amazon Linux Container Image<a name="amazon_linux_container_image"></a>

The Amazon Linux container image is built from the same software components that are included in the Amazon Linux AMI, and it is available for use in any environment as a base image for Docker workloads\. If you are already using the Amazon Linux AMI for applications in Amazon EC2, then you can easily containerize your applications with the Amazon Linux container image\.

You can use the Amazon Linux container image in your local development environment and then push your application to the AWS cloud using Amazon ECS\. For more information, see [Using Amazon ECR Images with Amazon ECS](ECR_on_ECS.md)\.

The Amazon Linux container image is available in Amazon ECR and on [Docker Hub](https://hub.docker.com/_/amazonlinux/)\. Support for the Amazon Linux container image can be found by visiting the [AWS developer forums](https://forums.aws.amazon.com/forum.jspa?forumID=228)\.

**To pull the Amazon Linux container image from Amazon ECR**

1. Authenticate your Docker client to the Amazon Linux container image Amazon ECR registry\. Authentication tokens are valid for 12 hours\. For more information, see [Registry Authentication](Registries.md#registry_auth)\. Specify the region that you would like to pull the image from \(if you are unsure, the `us-west-2` region used in the command below is fine\)\. If you do not use the `us-west-2` region for the following command, be sure to change the region in the subsequent commands and image tags\.
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr get-login --region us-west-2 --registry-ids 137112412989 --no-include-email
   ```

   Example output:

   ```
   docker login -u AWS -p password https://137112412989.dkr.ecr.us-west-2.amazonaws.com
   ```
**Important**  
If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   The resulting output is a docker login command that you use to authenticate your Docker client to the Amazon Linux container image Amazon ECR registry\.

1. Copy and paste the docker login command into a terminal to authenticate your Docker CLI to the registry\.
**Important**  
When you execute this docker login command, the command string can be visible by other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way and use them to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

1. \(Optional\) You can list the images within the Amazon Linux repository with the aws ecr list\-images command\. The `latest` tag always corresponds with the latest Amazon Linux container image that is available\.

   ```
   aws ecr list-images --region us-west-2 --registry-id 137112412989 --repository-name amazonlinux
   ```

1. Pull the Amazon Linux container image using the docker pull command\.

   ```
   docker pull 137112412989.dkr.ecr.us-west-2.amazonaws.com/amazonlinux:latest
   ```

1. \(Optional\) Run the container locally\.

   ```
   docker run -it 137112412989.dkr.ecr.us-west-2.amazonaws.com/amazonlinux:latest /bin/bash
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