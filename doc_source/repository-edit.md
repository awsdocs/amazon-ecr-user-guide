# Editing a repository<a name="repository-edit"></a>

Existing repositories can be edited to change its image tag mutability and image scanning settings\.

**To edit a repository**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to edit\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, select the repository to edit and choose **Edit**\.

1. For **Tag immutability**, choose the tag mutability setting for the repository\. Repositories configured with immutable tags will prevent image tags from being overwritten\. For more information, see [Image tag mutability](image-tag-mutability.md)\.

1. For **Scan on push**, choose the image scanning setting for the repository\. Repositories configured to scan on push will start an image scan whenever an image is pushed, otherwise image scans need to be started manually\. For more information, see [Image scanning](image-scanning.md)\.

1. Choose **Save** to update the repository settings\.