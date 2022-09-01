# Creating a lifecycle policy preview<a name="lpp_creation"></a>

A lifecycle policy preview provides a way see the impact of a lifecycle policy on an image repository before you apply it\. It is considered best practice to do a preview before applying a lifecycle policy to a repository\. The following procedure shows you how to create a lifecycle policy preview\.

**To create a lifecycle policy preview \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository on which to perform a lifecycle policy preview\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, on the **Private** tab, select a repository to view the repository image list\.

1. On the repository image list view, in the left navigation pane, choose **Lifecycle Policy**\.
**Note**  
If you don't see the **Lifecycle Policy** option in the navigation pane, ensure that you are in the repository image list view\.

1. On the repository lifecycle policy page, choose **Edit test rules**, **Create rule**\.

1. Enter the following details for each test lifecycle policy rule\.

   1. For **Rule priority**, type a number for the rule priority\.

   1. For **Rule description**, type a description for the lifecycle policy rule\.

   1. For **Image status**, choose **Tagged**, **Untagged**, or **Any**\.

   1. If you specified `Tagged` for **Image status**, then for **Tag prefixes**, you can optionally specify a list of image tags on which to take action with your lifecycle policy\. If you specified `Untagged`, this field must be empty\.

   1. For **Match criteria**, choose values for **Since image pushed** or **Image count more than** \(if applicable\)\.

   1. Choose **Save**\.

1. Create additional test lifecycle policy rules by repeating steps 5–7\.

1. To run the lifecycle policy preview, choose **Save and run test**\.

1. Under **Image matches for test lifecycle rules**, review the impact of your lifecycle policy preview\.

1. If you are satisfied with the preview results, choose **Apply as lifecycle policy** to create a lifecycle policy with the specified rules\. You should expect that after applying a lifecycle policy, the affected images are expired within 24 hours\.

1. If you aren't satisfied with the preview results, you may delete one or more test lifecycle rules and create one or more rules to replace them and then repeat the test\. If you don't apply the test lifecycle rules as a lifecycle policy, the test rules will persist in the console\.