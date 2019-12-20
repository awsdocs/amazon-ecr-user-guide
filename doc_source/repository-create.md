# Creating a Repository<a name="repository-create"></a>

Before you can push your Docker images to Amazon ECR, you must create a repository to store them in\. You can create Amazon ECR repositories with the AWS Management Console, or with the AWS CLI and AWS SDKs\.

**To create a repository**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose **Create repository**\.

1. For **Repository name**, enter a unique name for your repository\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags will prevent image tags from being overwritten\. For more information, see [Image Tag Mutability](image-tag-mutability.md)\.

1. For **Scan on push**, choose the image scanning setting for the repository\. Repositories configured to scan on push will start an image scan whenever an image is pushed, otherwise image scans need to be started manually\. For more information, see [Image Scanning](image-scanning.md)\.

1. Choose **Create repository**\.

1. \(Optional\) Select the repository you created and choose **View push commands** to view the steps to push an image to your new repository\.

   1. Retrieve the docker login command that you can use to authenticate your Docker client to your registry by pasting the aws ecr get\-login command from the console into a terminal window\. 
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. You can check your AWS CLI version with the aws \-\-version command\. If you are using Docker version 17\.06 or later, include the `--no-include-email` option after `get-login`\. If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   1. Run the docker login command that was returned in the previous step\. This command provides an authorization token that is valid for 12 hours\.
**Important**  
When you execute this docker login command, the command string can be visible to other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way\. They could use the credentials to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

   1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository\. Pasting the docker build command from the console into a terminal window\. Make sure that you are in the same directory as your Dockerfile\.

   1. Tag the image for your ECR registry and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step\. If you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

   1. Push the newly tagged image to your ECR repository by pasting the docker push command into a terminal window\.

   1. Choose **Close**\.