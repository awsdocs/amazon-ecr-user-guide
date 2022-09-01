# Viewing private repository details<a name="repository-info"></a>

After you created a repository, you can view details about the repository in the AWS Management Console:
+ Which images are stored in a repository
+ Details about each image stored in the repository, including the size and SHA digest for each image
+ The scan frequency specified for the contents of the repository
+ Whether the repository has an active pull through cache rule associated with it
+ The encryption setting for the repository

**Note**  
Starting with Docker version 1\.9, the Docker client compresses image layers before pushing them to a V2 Docker registry\. The output of the docker images command shows the uncompressed image size\. Therefore, keep in mind that Docker might return a larger image than the image shown in the AWS Management Console\.

**To view repository information \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to view\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the **Private** tab and then the repository to view\.

1. On the repository detail page, the console defaults to the **Images** view\. Use the navigation menu to view other information about the repository\.
   + Choose **Summary** to view the repository details and pull count data for the repository\.
   + Choose **Images** to view information about the image tags in the repository\. To view more information about the image, select the image tag\. For more information, see [Viewing image details](image-info.md)\.

     If there are untagged images that you want to delete, you can select the box to the left of the repositories to delete and choose **Delete**\. For more information, see [Deleting an image](delete_image.md)\.
   + Choose **Permissions** to view the repository policies that are applied to the repository\. For more information, see [Private repository policies](repository-policies.md)\.
   + Choose **Lifecycle Policy** to view the lifecycle policy rules that are applied to the repository\. The lifecycle events history is also viewed here\. For more information, see [Lifecycle policies](LifecyclePolicies.md)\.
   + Choose **Tags** to view the metadata tags that are applied to the repository\.