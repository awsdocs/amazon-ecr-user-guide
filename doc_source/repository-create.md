# Creating a private repository<a name="repository-create"></a>

Your container images are stored in Amazon ECR repositories\. Use the following steps to create a private repository using the AWSManagement Console\. For steps to create a repository using the AWS CLI, see [Step 3: Create a repository](getting-started-cli.md#cli-create-repository)\.

**To create a repository \(AWSManagement Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose **Create repository**\.

1. For **Repository name**, enter a unique name for your repository\. The repository name can be specified on its own \(for example `nginx-web-app`\)\. Alternatively, it can be prepended with a namespace to group the repository into a category \(for example `project-a/nginx-web-app`\)\.
**Note**  
The name must start with a letter and can only contain lowercase letters, numbers, hyphens \(\-\), underscores \(\_\), and forward slashes \(/\)\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags prevent image tags from being overwritten\. For more information, see [Image tag mutability](image-tag-mutability.md)\.

1. For **Scan on push**, choose the image scanning setting for the repository\. Repositories that are configured to scan on push start an image scan whenever an image is pushed\. If you want to start an image scan at a different time, you need to manually start the image scans\. For more information, see [Image scanning](image-scanning.md)\.

1. For **KMS encryption**, choose whether to enable encryption of the images in the repository using AWS Key Management Service\. By default, when KMS encryption is enabled, Amazon ECR uses an managed key \(KMS key\) with the alias `aws/ecr`\. This key is created in your account the first time that you create a repository with KMS encryption enabled\. For more information, see [Encryption at rest](encryption-at-rest.md)\.

1. When KMS encryption is enabled, select **Customer encryption settings \(advanced\)** to choose your own KMS key\. The KMS key must be in the same Region as the cluster\. Choose **Create an AWS KMS key** to navigate to the AWS KMS console to create your own key\.

1. Choose **Create repository**\.

1. \(Optional\) Select the repository that you created and choose **View push commands** to view the steps to push an image to your new repository\.

   1. Run the login command that authenticates your Docker client to your registry by using the command from the console in a terminal window\. This command provides an authorization token that is valid for 12 hours\.

   1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository\. Using the docker build command from the console in a terminal window\. Make sure that you are in the same directory as your Dockerfile\.

   1. Tag the image with your Amazon ECR registry URI and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step\. If you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

   1. Push the newly tagged image to your repository by using the docker push command in a terminal window\.

   1. Choose **Close**\.