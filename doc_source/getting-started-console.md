# Getting started with Amazon ECR using the AWS Management Console<a name="getting-started-console"></a>

Get started with Amazon ECR by creating a repository in the Amazon ECR console\. The Amazon ECR console guides you through the process to get started creating your first repository\.

Before you begin, be sure that you've completed the steps in [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md)\.

**To create an image repository**

A repository is where you store your Docker or Open Container Initiative \(OCI\) images in Amazon ECR\. Each time you push or pull an image from Amazon ECR, you specify the repository and the registry location which informs where to push the image to or where to pull it from\.

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. Choose **Get Started**\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags will prevent image tags from being overwritten\. For more information, see [Image tag mutability](image-tag-mutability.md)\.

1. For **Scan on push**, choose the image scanning setting for the repository\. Repositories configured to scan on push will start an image scan whenever an image is pushed, otherwise image scans need to be started manually\. For more information, see [Image scanning](image-scanning.md)\.

1. Choose **Create repository**\.

**Build, tag, and push a Docker image**

In this section of the wizard, you use the Docker CLI to tag an existing local image \(that you have built from a Dockerfile or pulled from another registry, such as Docker Hub\) and then push the tagged image to your Amazon ECR registry\. For more detailed steps on using the Docker CLI, see [Getting started with Amazon ECR using the AWS CLI](getting-started-cli.md)\.

1. Select the repository you created and choose **View push commands** to view the steps to push an image to your new repository\.

1. Run the login command that authenticates your Docker client to your registry by pasting the command from the console into a terminal window\. This command provides an authorization token that is valid for 12 hours\.

1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository\. Pasting the docker build command from the console into a terminal window\. Make sure that you are in the same directory as your Dockerfile\.

1. Tag the image with your Amazon ECR registry URI and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step\. If you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

1. Push the newly tagged image to your repository by pasting the docker push command into a terminal window\.

1. Choose **Close**\.