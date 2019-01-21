# Setting a Repository Policy Statement<a name="set-repository-policy"></a>

You can create and set an access policy statement for your repositories in the AWS Management Console by following the steps below\. You can create multiple policy statements per repository\. For example policies, see [Amazon ECR Repository Policy Examples](RepositoryPolicyExamples.md)\.

**Important**  
Amazon ECR users require permissions to call `ecr:GetAuthorizationToken` before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.

**To set a repository policy statement**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to set a policy statement on\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository to set a policy statement on\.

1. In the navigation pane, choose **Permissions**, **Edit**\.

1. On the **Edit permissions** page, choose **Add statement**\.

1. For **Statement name**, enter a name for the statement\.

1. For **Effect**, choose whether the policy statement should allow access or deny it\.

1. For **Principal**, choose the scope of users to apply the policy statement to\.
   + You can apply the statement to all authenticated AWS users by selecting the **Everyone \(\*\)** check box\.
   + You can apply the statement to all users under specific AWS accounts by listing those account numbers \(for example, 111122223333\) in the **AWS account number\(s\)** field\.
   + You can apply the statement to roles or users under your AWS account by checking the roles or users under the **IAM entities** list and choosing **>> Add** to move them to the **Selected IAM entities** list\.
**Note**  
For more complicated repository policies that are not currently supported in the AWS Management Console, you can apply the policy with the [https://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html](https://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html) AWS CLI command\.

1. For **Action**, choose the scope of the Amazon ECR API operations that the policy statement should apply to from the list of individual API operations\.

1. When you are finished, choose **Save** to set the policy\.
**Important**  
Amazon ECR users require permissions to call `ecr:GetAuthorizationToken` before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.

1. Repeat the previous step for each repository policy to add\.