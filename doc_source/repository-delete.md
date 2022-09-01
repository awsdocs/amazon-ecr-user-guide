# Deleting a private repository<a name="repository-delete"></a>

If you're finished using a repository, you can delete it\. When you delete a repository in the AWS Management Console, all of the images contained in the repository are also deleted; this cannot be undone\.

**To delete a repository \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to delete\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the **Private** tab and then select the repository to delete and choose **Delete**\.

1. In the **Delete *repository\_name*** window, verify that the selected repositories should be deleted and choose **Delete**\.
**Important**  
Any images in the selected repositories are also deleted\.