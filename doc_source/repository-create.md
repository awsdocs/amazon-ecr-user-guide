# Creating a repository<a name="repository-create"></a>

Before you can push your Docker images to Amazon ECR, you must create a repository to store them in\. You can create Amazon ECR repositories with the AWS Management Console, or with the AWS CLI and AWS SDKs\.

**To create a repository**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose **Create repository**\.

1. For **Repository name**, enter a unique name for your repository\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags will prevent image tags from being overwritten\. For more information, see [Image tag mutability](image-tag-mutability.md)\.

1. For **Scan on push**, choose the image scanning setting for the repository\. Repositories configured to scan on push will start an image scan whenever an image is pushed, otherwise image scans need to be started manually\. For more information, see [Image scanning](image-scanning.md)\.

1. For **KMS encryption**, choose whether to enable encryption of the images in the repository using AWS Key Management Service\. By default, when KMS encryption is enabled Amazon ECR uses an AWS managed customer master key \(CMK\) with alias `aws/ecr`, which is created in your account the first time that you create a repository with KMS encryption enabled\. For more information, see [Encryption at rest](encryption-at-rest.md)\.

1. When KMS encryption is enabled, select **Customer encryption settings \(advanced\)** to choose your own CMK\. The CMK must exist in the same Region as the cluster\. Choose **Create an AWS KMS key** to navigate to the AWS KMS console to create your own key\.

1. Choose **Create repository**\.

1. \(Optional\) Select the repository you created and choose **View push commands** to view the steps to push an image to your new repository\.

   1. Run the login command that authenticates your Docker client to your registry by pasting the command from the console into a terminal window\. This command provides an authorization token that is valid for 12 hours\.

   1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository\. Pasting the docker build command from the console into a terminal window\. Make sure that you are in the same directory as your Dockerfile\.

   1. Tag the image with your Amazon ECR registry URI and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step\. If you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

   1. Push the newly tagged image to your repository by pasting the docker push command into a terminal window\.

   1. Choose **Close**\.