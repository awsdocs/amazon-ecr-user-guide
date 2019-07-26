# Editing a Repository<a name="repository-edit"></a>

Existing repositories can be edited to change its image tag mutability setting\. When a repository is configured to be immutable, it will prevent image tags from being overwritten\. Once the repository is configured for immutable tags, an `ImageTagAlreadyExistsException` error will be returned if you attempt to push an image with a tag that already exists in the repository\. For more information, see [Image Tag Mutability](image-tag-mutability.md)\.

**To edit a repository**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to delete\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, select the repository to edit and choose **Edit**\.

1. For **Image tag mutability**, choose the tag mutability setting for the repository\.

1. Choose **Update** to update the repository settings\.