# Amazon ECR service\-linked role for replication<a name="slr-replication"></a>

## Service\-linked role permissions for Amazon ECR<a name="slr-permissions"></a>

Amazon ECR uses service\-linked roles named **AWSServiceRoleForECRReplication** – Allows Amazon ECR to replicate images across multiple accounts\.\.

The AWSServiceRoleForECRReplication service\-linked role trusts the following services to assume the role:
+ `replication.ecr.amazonaws.com`

The following `ECRReplicationServiceRolePolicy` role permissions policy allows Amazon ECR to use the following actions on resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:CreateRepository",
                "ecr:ReplicateImage"
            ],
            "Resource": "*"
        }
    ]
}
```

**Note**  
The `ReplicateImage` is an internal API that Amazon ECR uses for replication and can't be called directly\.

You must configure permissions to allow an IAM entity \(for example a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Amazon ECR<a name="create-slr"></a>

You don't need to manually create the Amazon ECR service\-linked role\. When you configure replication settings for your registry in the AWS Management Console, the AWS CLI, or the AWS API, Amazon ECR creates the service\-linked role for you\. 

If you delete this service\-linked role and need to create it again, you can use the same process to recreate the role in your account\. When you configure replication settings for your registry, Amazon ECR creates the service\-linked role for you again\. 

## Editing a service\-linked role for Amazon ECR<a name="edit-slr"></a>

Amazon ECR doesn't allow manually editing the AWSServiceRoleForECRReplication service\-linked role\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the service\-linked role for Amazon ECR<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way, you don’t have an unused entity that isn't actively monitored or maintained\. However, you must remove the replication configuration for your registry in every Region before you can manually delete the service\-linked role\.

**Note**  
If you try to delete resources while the Amazon ECR service is still using the roles, your delete action might fail\. If that happens, wait for a few minutes and try again\.

**To delete Amazon ECR resources used by the AWSServiceRoleForECRReplication**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, choose the Region your replication configuration is set on\.

1. In the navigation pane, choose **Registry settings**\.

1. Select both the **Cross\-Region replication** and **Cross\-account replication** settings\.

1. Choose **Save**\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForECRReplication service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.