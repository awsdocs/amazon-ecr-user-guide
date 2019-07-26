# Amazon ECR Repositories<a name="Repositories"></a>

Amazon Elastic Container Registry \(Amazon ECR\) provides API operations to create, monitor, and delete image repositories and set permissions that control who can access them\. You can perform the same actions in the **Repositories** section of the Amazon ECR console\. Amazon ECR also integrates with the Docker CLI allowing you to push and pull images from your development environments to your repositories\.

**Topics**
+ [Repository Concepts](#repository-concepts)
+ [Creating a Repository](repository-create.md)
+ [Viewing Repository Information](repository-info.md)
+ [Editing a Repository](repository-edit.md)
+ [Deleting a Repository](repository-delete.md)
+ [Amazon ECR Repository Policies](RepositoryPolicies.md)
+ [Tagging an Amazon ECR Repository](ecr-using-tags.md)

## Repository Concepts<a name="repository-concepts"></a>
+ By default, your account has read and write access to the repositories in your default registry \(`aws_account_id.dkr.ecr.region.amazonaws.com`\)\. However, IAM users require permissions to make calls to the Amazon ECR APIs and to push or pull images from your repositories\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.
+ Repositories can be controlled with both IAM user access policies and repository policies\. For more information, see [Amazon ECR Repository Policies](RepositoryPolicies.md)\.
+ Repository names can support namespaces, which you can use to group similar repositories\. For example if there are several teams using the same registry, Team A could use the `team-a` namespace while Team B uses the `team-b` namespace\. Each team could have their own image called `web-app`, but because they are each prefaced with the team namespace, the two images can be used simultaneously without interference\. Team A's image would be called `team-a/web-app`, while Team B's image would be called `team-b/web-app`\.