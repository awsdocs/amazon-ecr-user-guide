# Amazon ECR Repository Policies<a name="RepositoryPolicies"></a>

Amazon ECR uses resource\-based permissions to control access\. Resource\-based permissions let you specify who has access to a repository and what actions they can perform on it\. By default, only the repository owner has access to a repository\. You can apply a policy document that allows others to access your repository\.

**Important**  
Amazon ECR users require permissions to call `ecr:GetAuthorizationToken` before they can authenticate to a registry and push or pull any images from any Amazon ECR repository\. Amazon ECR provides several managed policies to control user access at varying levels; for more information, see [Amazon ECR Managed Policies](ecr_managed_policies.md)\.

**Topics**
+ [Setting a Repository Policy Statement](set-repository-policy.md)
+ [Deleting a Repository Policy Statement](delete-repository-policy.md)
+ [Amazon ECR Repository Policy Examples](RepositoryPolicyExamples.md)