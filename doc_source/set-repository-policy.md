# Setting a Repository Policy Statement<a name="set-repository-policy"></a>

You can create and set an access policy statement for your repositories in the AWS Management Console by following the steps below\. You can create multiple policy statements per repository\. 

**Important**  
Amazon ECR users require permissions to call `ecr:GetAuthorizationToken` before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.

**To set a repository policy statement**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the region that contains the repository to set a policy statement on\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository to set a policy statement on\.

1. On the **All repositories: *repository\_name*** page, choose **Permissions**, **Add**\.

1. For **Sid**, enter a description for what your policy statement does\.

1. For **Effect**, choose whether the policy statement should allow access or deny it\.

1. For **Principal**, choose the scope of users to apply the policy statement to\.

   + You can apply the statement to all authenticated AWS users by selecting the **Everybody** check box\.

   + You can apply the statement to all users under specific AWS accounts by listing those accounts in the **AWS account number\(s\)** field\.

   + You can apply the statement to roles or users under your AWS account by checking the roles or users under the **All IAM entities** list and choosing **>> Add** to move them to the **Selected IAM entities** list\.
**Note**  
For more complicated repository policies that are not currently supported in the AWS Management Console, you can apply the policy with the [set\-repository\-policy](http://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html) AWS CLI command\.

1. For **Action**, choose the scope of the Amazon ECR API operations that the policy statement should apply to\. You can choose individual API operations, or you can choose from the preset task\-based options\.

   + **All actions** sets the scope to all Amazon ECR API operations\.

   + **Push/Pull actions** sets the scope to Amazon ECR API operations required to push or pull images in this repository with the Docker CLI\.

   + **Pull only actions** sets the scope to Amazon ECR API operations required only to pull images from this repository with the Docker CLI\.

1. When you are finished, choose **Save** to set the policy\.
**Important**  
Amazon ECR users require permissions to call `ecr:GetAuthorizationToken` before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.