# Creating a Lifecycle Policy Preview<a name="lpp_creation"></a>

A lifecycle policy preview allows you to see the impact of a lifecycle policy on an image repository before you execute it\. The following procedure shows you how to create a lifecycle policy preview\.

**To create a lifecycle policy preview using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the Region that contains the repository on which to perform a lifecycle policy preview\.

1. In the navigation pane, choose **Repositories** and select a repository\.

1. On the **All repositories: *repository\_name*** page, choose **Dry\-Run Lifecycle Rules**, **Add**\.

1. Enter the following details for your lifecycle policy rule:

   1. For **Rule Priority**, type a number for the rule priority\.

   1. For **Rule Description**, type a description for the lifecycle policy rule\.

   1. For **Image Status**, choose **Tagged**, **Untagged**, or **Any**\.

   1. If you specified `Tagged` for **Image Status**, then for **Tag Prefix List**, you can optionally specify a list of image tags on which to take action with your lifecycle policy\. If you specified `Untagged`, this field must be empty\.

   1. For **Match criteria**, choose values for **Count Type**, **Count Number**, and **Count Unit** \(if applicable\)\.

1. Choose **Save**\.

1. Create additional lifecycle policy rules by repeating steps 5–7\.

1. To run the lifecycle policy preview, choose **Save and preview results**\.

1. Under **Preview Image Results**, review the impact of your lifecycle policy preview\.

1. If you are satisfied with the preview results, choose **Apply as lifecycle policy** to create a lifecycle policy with the specified rules\.

**Note**  
You should expect that after creating a lifecycle policy, the affected images are expired within 24 hours\.