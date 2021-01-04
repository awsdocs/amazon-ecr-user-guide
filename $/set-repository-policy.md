# Setting a repository policy statement<a name="set-repository-policy"></a>

You can add an access policy statement to a repository in the AWS Management Console by following the steps below\. You can add multiple policy statements per repository\. For example policies, see [Repository policy examples](repository-policy-examples.md)\.

**Important**  
Amazon ECR requires that users have permission to make calls to the `ecr:GetAuthorizationToken` API through an IAM policy before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed IAM policies to control user access at varying levels; for more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

**To set a repository policy statement**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository to set a policy statement on\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository to set a policy statement on\.

1. In the navigation pane, choose **Permissions**, **Edit**\.

1. On the **Edit permissions** page, choose **Add statement**\.

1. For **Statement name**, enter a name for the statement\.

1. For **Effect**, choose whether the policy statement will result in an allow or an explicit deny\.

1. For **Principal**, choose the scope to apply the policy statement to\. For more information, see [AWS JSON Policy Elements: Principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) in the *IAM User Guide*\.
   + You can apply the statement to all authenticated AWS users by selecting the **Everyone \(\*\)** check box\.
   + For **Service principal**, specify the service principal name \(for example, `ecs.amazonaws.com`\) to apply the statement to a specific service\.
   + For **AWS Account IDs**, specify an AWS account number \(for example, `111122223333`\) to apply the statement to all users under a specific AWS account\. Multiple accounts can be specified by using a comma delimited list\.
   + For **IAM Entities**, select the roles or users under your AWS account to apply the statement to\.
**Note**  
For more complicated repository policies that are not currently supported in the AWS Management Console, you can apply the policy with the [https://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html](https://docs.aws.amazon.com/cli/latest/reference/ecr/set-repository-policy.html) AWS CLI command\.

1. For **Actions**, choose the scope of the Amazon ECR API operations that the policy statement should apply to from the list of individual API operations\.

1. When you are finished, choose **Save** to set the policy\.

1. Repeat the previous step for each repository policy to add\.