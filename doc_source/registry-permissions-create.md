# Setting a private registry permission statement<a name="registry-permissions-create"></a>

You can add or update the permissions policy for your registry by using the following steps\. You can add multiple policy statements per registry\. For example policies, see [Private registry policy examples](registry-permissions-examples.md)\.

**Topics**
+ [Private registry permissions for replication](#registry-permissions-create-replication)
+ [Private registry permissions for pull through cache](#registry-permissions-create-pullthroughcache)

## Private registry permissions for replication<a name="registry-permissions-create-replication"></a>

The cross account policy type is used to grant permissions to an AWS principal, allowing the replication of the repositories from a source registry to your registry\. By default, you have permission to configure cross\-Region replication within your own registry\. You only need to configure the registry policy if you're granting another account permission to replicate contents to your registry\.

A registry policy must grant permission for the `ecr:ReplicateImage` API action\. This API is an internal Amazon ECR API that can replicate images between Regions or accounts\. You can also grant permission for the `ecr:CreateRepository` permission, which allows Amazon ECR to create repositories in your registry if they don't exist already\. If the `ecr:CreateRepository` permission isn't provided, a repository with the same name as the source repository must be created manually in your registry\. If neither is done, replication fails\. Any failed CreateRepository or ReplicateImage API actions show up in CloudTrail\.

### To configure a permissions policy for replication \(AWS Management Console\)<a name="registry-permissions-create-console"></a>

**To configure a replication permissions policy for a private registry \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, choose the Region to configure your registry policy in\.

1. In the navigation pane, choose **Private registry**, **Registry permissions**\.

1. On the **Registry permissions** page, choose **Generate statement**\.

1. Complete the following steps to define your policy statement using the policy generator\.

   1. For **Policy type**, choose **Cross account policy**\.

   1. For **Statement ID**, enter a unique statement ID\. This field is used as the `Sid` on the registry policy\.

   1. For **Accounts**, enter the account IDs for each account you want to grant permissions to\. When specifying multiple account IDs, separate them with a comma\.

1. Expand the **Preview policy statement** section to review the registry permissions policy statement\.

1. After the policy statement is confirmed, choose **Add to policy** to save the policy to your registry\.

### To configure a permissions policy for replication \(AWS CLI\)<a name="registry-permissions-create-cli"></a>

**To configure a permissions policy for a private registry \(AWS CLI\)**

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

## Private registry permissions for pull through cache<a name="registry-permissions-create-pullthroughcache"></a>

Amazon ECR private registry permissions may be used to scope the permissions of individual IAM entities to use pull through cache\. If an IAM entity has more permissions granted by an IAM policy than the registry permissions policy is granting, the IAM policy takes precedence\.

**To create a private registry permissions policy \(AWS Management Console\)**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, choose the Region to configure your private registry permissions statement in\.

1. In the navigation pane, choose **Private registry**, **Registry permissions**\.

1. On the **Registry permissions** page, choose **Generate statement**\.

1. For each pull through cache permissions policy statement you want to create, do the following\.

   1. For **Policy type**, choose **Pull through cache policy**\.

   1. For **Statement id**, provide a name for the pull through cache statement policy\.

   1. For **IAM entities**, specify the IAM users, groups, or roles to include in the policy\.

   1. For **Repository namespace**, select the pull through cache rule to associate the policy with\.

   1. For **Repository names**, specify the repository base name to apply the rule for\. For example, if you want to specify the Amazon Linux repository on Amazon ECR Public, the repository name would be `amazonlinux`\.