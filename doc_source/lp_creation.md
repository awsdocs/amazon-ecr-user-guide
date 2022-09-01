# Creating a lifecycle policy<a name="lp_creation"></a>

A lifecycle policy allows you to create a set of rules that expire unused repository images\. The following procedure shows you how to create a lifecycle policy\. You should expect that after creating a lifecycle policy, the affected images are expired within 24 hours\.

**Important**  
It is considered best practice to create a lifecycle policy preview to ensure that the images affected by your lifecycle policy rules are what you intend\. For more information, see [Creating a lifecycle policy preview](lpp_creation.md)\.

## To create a lifecycle policy \(AWS Management Console\)<a name="lp-creation-console"></a>

**To create a lifecycle policy using the console**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository for which to create a lifecycle policy\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, on the **Private** tab, select a repository to view the repository image list\.

1. On the repository image list view, in the left navigation pane, choose **Lifecycle Policy**\.
**Note**  
If you don't see the **Lifecycle Policy** option in the navigation pane, ensure that you are in the repository image list view\.

1. On the repository lifecycle policy page, choose **Create rule**\.

1. Enter the following details for your lifecycle policy rule\.

   1. For **Rule priority**, type a number for the rule priority\.

   1. For **Rule description**, type a description for the lifecycle policy rule\.

   1. For **Image status**, choose **Tagged**, **Untagged**, or **Any**\.

   1. If you specified `Tagged` for **Image status**, then for **Tag prefixes**, you can optionally specify a list of image tags on which to take action with your lifecycle policy\. If you specified `Untagged`, this field must be empty\.

   1. For **Match criteria**, choose values for **Since image pushed** or **Image count more than** \(if applicable\)\.

   1. Choose **Save**\.

1. Create additional lifecycle policy rules by repeating steps 5–7\.

## To create a lifecycle policy \(AWS CLI\)<a name="lp-creation-cli"></a>

**To create a lifecycle policy using the AWS CLI**

1. Obtain the name of the repository for which to create the lifecycle policy\.

   ```
   aws ecr describe-repositories
   ```

1. Create a local file named `policy.json` with the contents of the lifecycle policy\. For lifecycle policy examples, see [Examples of lifecycle policies](lifecycle_policy_examples.md)\.

1. Create a lifecycle policy by specifying the repository name and reference the lifecycle policy JSON file you created\.

   ```
   aws ecr put-lifecycle-policy \
         --repository-name repository-name \
         --lifecycle-policy-text file://policy.json
   ```