# Creating a lifecycle policy preview<a name="lpp_creation"></a>

A lifecycle policy preview allows you to see the impact of a lifecycle policy on an image repository before you execute it\. The following procedure shows you how to create a lifecycle policy preview\.

**To create a lifecycle policy preview using the console**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region that contains the repository on which to perform a lifecycle policy preview\.

1. In the navigation pane, choose **Repositories** and select a repository\.

1. On the **Repositories: *repository\_name*** page, in the navigation pane choose **Lifecycle Policy**\.

1. On the **Repositories: *repository\_name*: Lifecycle policy** page, choose **Edit test rules**, **Create rule**\.

1. Enter the following details for your lifecycle policy rule:

   1. For **Rule priority**, type a number for the rule priority\.

   1. For **Rule description**, type a description for the lifecycle policy rule\.

   1. For **Image status**, choose **Tagged**, **Untagged**, or **Any**\.

   1. If you specified `Tagged` for **Image status**, then for **Tag prefixes**, you can optionally specify a list of image tags on which to take action with your lifecycle policy\. If you specified `Untagged`, this field must be empty\.

   1. For **Match criteria**, choose values for **Since image pushed** or **Image count more than** \(if applicable\)\.

1. Choose **Save**\.

1. Create additional lifecycle policy rules by repeating steps 5–7\.

1. To run the lifecycle policy preview, choose **Save and run test**\.

1. Under **Image matches for test lifecycle rules**, review the impact of your lifecycle policy preview\.

1. If you are satisfied with the preview results, choose **Apply as lifecycle policy** to create a lifecycle policy with the specified rules\.

**Note**  
You should expect that after creating a lifecycle policy, the affected images are expired within 24 hours\.