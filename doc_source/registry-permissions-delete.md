# Deleting a private registry permission statement<a name="registry-permissions-delete"></a>

You can delete all permissions policy statements for your registry by using the following steps\.

**To delete a permissions policy for a private registry \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, choose the Region to configure your registry permissions policy in\.

1. In the navigation pane, choose **Registries**\.

1. On the **Registries** page, select your **Private** registry and choose **Permissions**\.

1. On the **Private registry permissions** page, choose **Delete**\.

1. On the **Delete registry policy** confirmation screen, choose **Delete policy**\.

**To delete a permissions policy for a private registry \(AWS CLI\)**

1. Delete the registry policy\.

   ```
   aws ecr delete-registry-policy \
        --region us-west-2
   ```

1. Retrieve the policy for your registry to confirm\.

   ```
   aws ecr get-registry-policy \
        --region us-west-2
   ```