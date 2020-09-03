# Deleting an image<a name="delete_image"></a>

If you are done using an image, you can delete it from your repository\. You can delete an image using the AWS Management Console, or the AWS CLI\.

**Note**  
If you are done with a repository, you can delete the entire repository and all of the images within it\. For more information, see [Deleting a repository](repository-delete.md)\.

**To delete an image with the AWS Management Console**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the image to delete\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository that contains the image to delete\.

1. On the **Repositories: *repository\_name*** page, select the box to the left of the image to delete and choose **Delete**\.

1. In the **Delete image\(s\)** dialog box, verify that the selected images should be deleted and choose **Delete**\.

**To delete an image with the AWS CLI**

1. List the images in your repository so that you can identify them by image tag or digest\. 

   ```
   aws ecr list-images --repository-name my-repo
   ```

1. \(Optional\) Delete any unwanted tags for the image by specifying the tag of the image you want to delete\.
**Note**  
When you delete the last tag for an image, the image is deleted\.

   ```
   aws ecr batch-delete-image --repository-name my-repo --image-ids imageTag=latest
   ```

1. Delete the image by specifying the digest of the image to delete\.
**Note**  
When you delete an image by referencing its digest, the image and all of its tags are deleted\.

   ```
   aws ecr batch-delete-image --repository-name my-repo --image-ids imageDigest=sha256:4f70ef7a4d29e8c0c302b13e25962d8f7a0bd304c7c2c1a9d6fa3e9de6bf552d
   ```