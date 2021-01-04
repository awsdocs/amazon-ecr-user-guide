# Setting a private registry permission statement<a name="registry-permissions-create"></a>

You can add or update the permission policy for your registry by using the following steps\. You can add multiple policy statements per registry\. For example policies, see [Private registry policy examples](registry-permissions-examples.md)\.

**To configure a permission policy for a private registry \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, choose the Region to configure your registry policy in\.

1. In the navigation pane, choose **Registries**\.

1. On the **Registries** page, select your **Private** registry and choose **Permissions**\.

1. On the **Private registry permissions** page, choose **Generate statement**\.

1. Complete the following steps to define your policy statement using the policy generator\.

   1. For **Policy type**, choose **Cross\-account policy**\.

   1. For **Statement ID**, enter a unique statement ID\. This field is used as the `Sid` on the registry policy\.

   1. For **Accounts**, enter the account IDs for each account you want to grant permissions to\. When specifying multiple account IDs, separate them with a comma\.

1. Expand the **Preview policy statement** section to review the registry permissions policy statement\.

1. After the policy statement is confirmed, choose **Add to policy** to save the policy to your registry\.

**To configure a permission policy for a private registry \(AWS CLI\)**

1. Create a file named `registry_policy.json` and populate it with a registry policy\.

   ```
   {
       "Version":"2012-10-17",
       "Statement":[
           {
               "Sid":"ReplicationAccessCrossAccount",
               "Effect":"Allow",
               "Principal":{
                   "AWS":"arn:aws:iam::source_account_id:root"
               },
               "Action":[
                   "ecr:CreateRepository",
                   "ecr:ReplicateImage"
               ],
               "Resource": [
                   "arn:aws:ecr:us-west-2:your_account_id:repository/*"
               ]
           }
       ]
   }
   ```

1. Create the registry policy using the policy file\.

   ```
   aws ecr put-registry-policy \
         --policy-text file://registry_policy.json \
         --region us-west-2
   ```

1. Retrieve the policy for your registry to confirm\.

   ```
   aws ecr get-registry-policy \
         --region us-west-2
   ```