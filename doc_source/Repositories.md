# Amazon ECR private repositories<a name="Repositories"></a>

Amazon Elastic Container Registry \(Amazon ECR\) provides API operations to create, monitor, and delete image repositories and set permissions that control who can access them\. You can perform the same actions in the **Repositories** section of the Amazon ECR console\. Amazon ECR also integrates with the Docker CLI, so that you push and pull images from your development environments to your repositories\.

**Topics**
+ [Private repository concepts](#repository-concepts)
+ [Creating a private repository](repository-create.md)
+ [Viewing private repository details](repository-info.md)
+ [Editing a private repository](repository-edit.md)
+ [Deleting a private repository](repository-delete.md)
+ [Private repository policies](repository-policies.md)
+ [Tagging a private repository](ecr-using-tags.md)

## Private repository concepts<a name="repository-concepts"></a>
+ By default, your account has read and write access to the repositories in your default registry \(`aws_account_id.dkr.ecr.region.amazonaws.com`\)\. However, IAM users require permissions to make calls to the Amazon ECR APIs and to push or pull images to and from your repositories\. Amazon ECR provides several managed policies to control user access at varying levels\. For more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.
+ Repositories can be controlled with both IAM user access policies and individual repository policies\. For more information, see [Private repository policies](repository-policies.md)\.
+ Repository names can support namespaces, which you can use to group similar repositories\. For example, if there are several teams using the same registry, Team A can use the `team-a` namespace, and Team B can use the `team-b` namespace\. By doing this, each team has their own image called `web-app` with each image prefaced with the team namespace\. This configuration allows these images on each team to be used simultaneously without interference\. Team A's image is `team-a/web-app`, and Team B's image is `team-b/web-app`\.
+ Your images can be replicated to other repositories across Regions in your own registry and across accounts\. You can do this by specifying a replication configuration in your registry settings\. For more information, see [Private registry settings](registry-settings.md)\.