# Creating a Lifecycle Policy<a name="lp_creation"></a>

A lifecycle policy allows you to create a set of rules that expire unused repository images\. The following procedure shows you how to create a lifecycle policy using the console\. You should expect that after creating a lifecycle policy the affected images will be expired within 24 hours\.

**Setting up a lifecycle policy using the AWS CLI**

1. Obtain the ID for the repository for which to create the lifecycle policy:

   ```
   aws ecr describe-repositories
   ```

1. Create a lifecycle policy:

   ```
   aws ecr put-lifecycle-policy [--registry-id <string>] --repository-name <string> --policy-text <string>
   ```

**Setting up a lifecycle policy preview using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the region that contains the repository on which to perform a lifecycle policy preview\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository on which to perform a lifecycle policy preview\.

1. On the **All repositories: *repository\_name*** page, choose **Lifecycle Policy**, **Add**\.

1. Enter the following details for your lifecycle policy rule:

   1. For **Rule Priority**, type a number for the rule priority\.

   1. For **Rule Description**, type a description for the lifecycle policy rule\.

   1. For **Image Status**, choose either **Tagged** or **Untagged**\.

   1. If you specified `Tagged` for **Image Status**, for **Tag Prefix List**, you can optionally specify a list of image tags on which to take action with your lifecycle policy\. If you specified `Untagged`, this field must be empty\.

   1. For **Match criteria**, choose values for **Count Type**, **Count Number**, and **Count Unit** \(if applicable\)\.

1. Choose **Apply as lifecycle policy**\.
**Note**  
If you choose **Dry Run**, it creates a lifecycle policy preview\.