# Getting Started with Amazon ECR<a name="ECR_GetStarted"></a>

Get started with Amazon Elastic Container Registry \(Amazon ECR\) by creating a repository in the Amazon ECS console\. The Amazon ECR first run wizard guides you through the process to get started with Amazon ECR\.

**Important**  
Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECR](get-set-up-for-amazon-ecr.md)\.

**Configure repository**

A repository is where you store Docker images in Amazon ECR\. Every time you push or pull an image from Amazon ECR, you specify the registry and repository location to tell Docker where to push the image to or where to pull it from\.

1. Open the Amazon ECS console repositories page at [https://console\.aws\.amazon\.com/ecs/home\#/repositories](https://console.aws.amazon.com/ecs/home#/repositories)\.

1. For **Repository name**, enter a unique name for your repository and choose **Next step**\.

**Build, tag, and push Docker image**

In this section of the wizard, you use the Docker CLI to tag an existing local image \(that you have built from a Dockerfile or pulled from another registry, such as Docker Hub\) and then push the tagged image to your Amazon ECR registry\.

1. Retrieve the docker login command that you can use to authenticate your Docker client to your registry by pasting the aws ecr get\-login command from the console into a terminal window\. 
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. You can check your AWS CLI version with the aws \-\-version command\. If you are using Docker version 17\.06 or later, include the `--no-include-email` option after `get-login`\. If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

1. Run the docker login command that was returned in the previous step\. This command provides an authorization token that is valid for 12 hours\.
**Important**  
When you execute this docker login command, the command string can be visible by other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way and use them to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository by pasting the docker build command from the console into a terminal window\. Make sure you are in the same directory as your Dockerfile\.

1. Tag the image for your ECR registry and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step; if you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

1. Push the newly tagged image to your ECR repository by pasting the docker push command into a terminal window\.

1. Choose **Done**\.