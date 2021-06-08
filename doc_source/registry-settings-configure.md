# Configuring private image replication<a name="registry-settings-configure"></a>

Replication settings are configured separately for each Region\. Use the following steps to configure replication for your private registry\.

**To configure registry replication settings \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to configure your registry replication settings for\.

1. In the navigation pane, choose **Registries**\.

1. On the **Registries** page, select your **Private** registry and choose **Edit**\.

1. On the **Edit registry** page, do the following\.

   1. For **Cross\-Region replication**, choose the cross\-Region replication setting for the registry\. If set to **Enabled**, choose one or more **Destination regions**\.

   1. For **Cross\-account replication**, choose the cross\-account replication setting for the registry\. If set to **Enabled**, enter the account ID for the destination account and one or more **Destination regions** to replicate to\.
**Important**  
For cross\-account replication to occur, the destination account must configure a registry permissions policy to allow replication to occur\. For more information, see [Private registry permissions](registry-permissions.md)\.

1. Choose **Save**\.

**To configure registry replication settings \(AWS CLI\)**

1. Create a JSON file containing the replication configuration settings to define for your registry\. This might contain one or more rules, with each rule containing a destination Region and account\. If you want to replicate the images in your own registry between Regions, then specify your own account ID\. For more examples, see [Private image replication examples](registry-settings-examples.md)\.

   ```
   {
       "rules": [
           {
               "destinations": [
                   {
                       "region": "destination_region",
                       "registryId": "destination_accountId"
                   }
               ]
           }
       ]
   }
   ```

1. Create a replication configuration for your registry\.

   ```
   aws ecr put-replication-configuration \
        --replication-configuration file://crr-setup.json \
        --region us-west-2
   ```

1. Confirm your registry settings\.

   ```
   aws ecr describe-registry \
        --region us-west-2
   ```