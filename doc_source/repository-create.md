# Creating a Repository<a name="repository-create"></a>

Before you can push your Docker images to Amazon ECR, you need to create a repository to store them in\. You can create Amazon ECR repositories with the AWS Management Console, or with the AWS CLI and AWS SDKs\.

**To create a repository**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose **Create repository**\.

1. For **Repository name**, enter a unique name for your repository and choose **Next step**\.

1. \(Optional\) On the **Build, tag, and push Docker image** page, complete the following steps to push an image to your new repository\. If you do not want to push an image at this time, you can choose **Done** to finish\.

   1. Retrieve the docker login command that you can use to authenticate your Docker client to your registry by pasting the aws ecr get\-login command from the console into a terminal window\. 
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. You can check your AWS CLI version with the aws \-\-version command\. If you are using Docker version 17\.06 or later, include the `--no-include-email` option after `get-login`\. If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   1. Run the docker login command that was returned in the previous step\. This command provides an authorization token that is valid for 12 hours\.
**Important**  
When you execute this docker login command, the command string can be visible to other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way and use them to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

   1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository by pasting the docker build command from the console into a terminal window\. Make sure you are in the same directory as your Dockerfile\.

   1. Tag the image for your ECR registry and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step; if you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

   1. Push the newly tagged image to your ECR repository by pasting the docker push command into a terminal window\.

   1. Choose **Done**\.