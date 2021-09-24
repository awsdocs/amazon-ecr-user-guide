# Viewing replication status<a name="replication-status"></a>

The replication status of an individual container image can be viewed by querying using either the image tag or image digest\.

**Checking replication status \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that is the source of your replicated registry\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository to check the replication status of\.

1. On the repository details page, choose the **Image tag** to check the replication status of\.

1. For **Image replication status**, verify the replication status\. You can view the replication status based on the image tag or image digest\.

**Checking replication status \(AWS CLI\)**
+ The replication status of the contents of a repository can be viewed based on the image tag using the following command\.

  ```
  aws ecr describe-image-replication-status \
       --repository-name repository_name \
       --image-id imageTag=image_tag \
       --region us-west-2
  ```
+ The replication status of the contents of a repository can be viewed based on the image digest using the following command\.

  ```
  aws ecr describe-image-replication-status \
       --repository-name repository_name \
       --image-id imageDigest=image_digest \
       --region us-west-2
  ```