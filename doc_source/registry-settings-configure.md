# Configuring private image replication<a name="registry-settings-configure"></a>

Replication settings are configured separately for each Region\. Use the following steps to configure replication for your private registry\.

**To configure registry replication settings \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/repositories](https://console.aws.amazon.com/ecr/repositories)\.

1. From the navigation bar, choose the Region to configure your registry replication settings for\.

1. In the navigation pane, choose **Private registry**\.

1. On the **Private registry** page, on the **Replication** section, choose **Edit**\.

1. On the **Replication** page, choose **Add replication rule**\.

1. On the **Destination types** page, choose whether to enable cross\-Region replication, cross\-account replication, or both and then choose **Next**\.

1. If cross\-Region replication is enabled, then for **Configure destination regions**, choose one or more **Destination regions** and then choose **Next**\.

1. If cross\-account replication is enabled, then for **Cross\-account replication**, choose the cross\-account replication setting for the registry\. For **Destination account**, enter the account ID for the destination account and one or more **Destination regions** to replicate to\. Choose **Destination account \+** to configure additional accounts as replication destinations\.
**Important**  
For cross\-account replication to occur, the destination account must configure a registry permissions policy to allow replication to occur\. For more information, see [Private registry permissions](registry-permissions.md)\.

1. \(Optional\) On the **Add filters** page, specify one or more filters for the replication rule and then choose **Add**\. Repeat this step for each filter you want to associate with the replication action\. Filters are specified as repository name prefixes\. If no filters are specified, all images are replicated\. Choose **Next** once all filters have been added\.

1. On the **Review and submit** page, review the replication rule configuration and then choose **Submit rule**\.

**To configure registry replication settings \(AWS CLI\)**

1. Create a JSON file containing the replication rules to define for your registry\. A replication configuration may contain up to 10 rules, with up to 25 unique destinations across all rules and 100 filters per each rule\. To configure cross\-Region replication within your own account, you specify your own account ID\. For more examples, see [Private image replication examples](registry-settings-examples.md)\.

   ```
   {
   	"rules": [{
   		"destinations": [{
   			"region": "destination_region",
   			"registryId": "destination_accountId"
   		}],
   		"repositoryFilters": [{
   			"filter": "repository_prefix_name",
   			"filterType": "PREFIX_MATCH"
   		}]
   	}]
   }
   ```

1. Create a replication configuration for your registry\.

   ```
   aws ecr put-replication-configuration \
        --replication-configuration file://replication-settings.json \
        --region us-west-2
   ```

1. Confirm your registry settings\.

   ```
   aws ecr describe-registry \
        --region us-west-2
   ```