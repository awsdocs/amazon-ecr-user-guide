# Viewing Repository Information<a name="repository-info"></a>

After you have created a repository, you can view its information in the AWS Management Console\. You can see which images are stored in a repository, whether or not an image is tagged and what the tags for the image are, the SHA digest for the images, the size of the images in MiB, and when the image was pushed to the repository\. 

**Note**  
Beginning with Docker version 1\.9, the Docker client compresses image layers before pushing them to a V2 Docker registry\. The output of the docker images command shows the uncompressed image size, so it may return a larger image size than the image sizes shown in the AWS Management Console\.

You can also view the repository policies that are applied to the repository\.

![\[Viewing repository information\]](http://docs.aws.amazon.com/AmazonECR/latest/userguide/images/repository_info.png)

**To view repository information**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the region that contains the repository to view\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository to view\.

1. On the **All repositories : *repository\_name*** page, choose the tab which corresponds to the information you would like to view\.
   + Choose the **Images** tab to view information about the images in the repository\. If there are untagged images that you would like to delete, you can select the box to the left of the repositories to delete and choose **Delete**\. For more information, see [Deleting an Image](delete_image.md)\.
   + Choose the **Permissions** tab to view the repository policies that are applied to the repository\. For more information, see [Amazon ECR Repository Policies](RepositoryPolicies.md)\.