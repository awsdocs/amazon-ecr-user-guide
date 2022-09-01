# Creating a private repository<a name="repository-create"></a>

Your container images are stored in Amazon ECR repositories\. Use the following steps to create a private repository using the AWS Management Console\. For steps to create a repository using the AWS CLI, see [Step 3: Create a repository](getting-started-cli.md#cli-create-repository)\.

**To create a repository \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to create your repository in\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the **Private** tab, and then choose **Create repository**\.

1. For **Visibility settings**, verify that **Private** is selected\.

1. For **Repository name**, enter a unique name for your repository\. The repository name can be specified on its own \(for example `nginx-web-app`\)\. Alternatively, it can be prepended with a namespace to group the repository into a category \(for example `project-a/nginx-web-app`\)\. The repository name must start with a letter and can only contain lowercase letters, numbers, hyphens, underscores, and forward slashes\. Using a double hyphen, underscore, or forward slash isn't supported\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags prevent image tags from being overwritten\. For more information, see [Image tag mutability](image-tag-mutability.md)\.

1. For **Scan on push**, while you can specify the scan settings at the repository level for basic scanning, it is best practice to specify the scan configuration at the private registry level\. Specify the scanning settings at the private registry allow you to enable either enhanced scanning or basic scanning as well as define filters to specify which repositories are scanned\. For more information, see [Image scanning](image-scanning.md)\.

1. For **KMS encryption**, choose whether to enable encryption of the images in the repository using AWS Key Management Service\. By default, when KMS encryption is enabled, Amazon ECR uses an AWS managed key \(KMS key\) with the alias `aws/ecr`\. This key is created in your account the first time that you create a repository with KMS encryption enabled\. For more information, see [Encryption at rest](encryption-at-rest.md)\.

1. When KMS encryption is enabled, select **Customer encryption settings \(advanced\)** to choose your own KMS key\. The KMS key must be in the same Region as the cluster\. Choose **Create an AWS KMS key** to navigate to the AWS KMS console to create your own key\.

1. Choose **Create repository**\.

1. \(Optional\) Select the repository that you created and choose **View push commands** to view the steps to push an image to your new repository\. For more information about pushing an image to your repository, see [Pushing an image](image-push.md)\.