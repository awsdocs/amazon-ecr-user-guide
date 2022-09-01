# Editing a private repository<a name="repository-edit"></a>

Existing repositories can be edited to change its image tag mutability and image scanning settings\.

**To edit a repository \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to edit\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the **Private** tab and then select the repository to edit and choose **Edit**\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags prevent image tags from being overwritten\. For more information, see [Image tag mutability](image-tag-mutability.md)\.

1. For **Image scan settings**, while you can specify the scan settings at the repository level for basic scanning, it is best practice to specify the scan configuration at the private registry level\. Specify the scanning settings at the private registry allow you to enable either enhanced scanning or basic scanning as well as define filters to specify which repositories are scanned\. For more information, see [Image scanning](image-scanning.md)\.

1. For **Encryption settings**, this is a view only field as the encryption settings for a repository can't be changed once the repository is created\.

1. Choose **Save** to update the repository settings\.