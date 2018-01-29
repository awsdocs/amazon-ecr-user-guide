# Creating Amazon ECR IAM Policies<a name="ECR_IAM_user_policies"></a>

You can create specific IAM policies to restrict the calls and resources that users in your account have access to, and then attach those policies to IAM users\.

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\. For more general information about IAM policies, see [Permissions and Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. For more information about managing and creating custom IAM policies, see [Managing IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html)\.

**To create an IAM policy for a user**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, **Create Policy**\. 

1. In the **Create Policy** section, choose **Select** next to **Create Your Own Policy**\.

1. For **Policy Name**, type your own unique name, such as `AmazonECRUserPolicy`\.

1. For **Policy Document**, paste the policy to apply to the user\. You can use the [Amazon ECR Managed Policies](ecr_managed_policies.md) as a starting point to create your own more or less restrictive IAM policies to use with Amazon ECR\.

1. Choose **Create Policy** to finish\. 

**To attach an IAM policy to a user**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users** and then choose the user to attach the policy to\. 

1. Choose **Permissions**, **Add permissions**\.

1. In the **Grant permissions** section, choose **Attach existing policies directly**\.

1. Select the custom policy that you created in the previous procedure and choose **Next: Review**\.

1. Review your details and choose **Add permissions** to finish\.