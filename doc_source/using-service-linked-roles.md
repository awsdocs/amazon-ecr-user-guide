# Using service\-linked roles for Amazon ECR<a name="using-service-linked-roles"></a>

Amazon Elastic Container Registry \(Amazon ECR\) uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) to provide access to replicate resources\. A service\-linked role is a unique type of IAM role that is linked directly to Amazon ECR\. The service\-linked role is predefined by Amazon ECR\. It includes all of the permissions that the service requires to support cross\-Region and cross\-account image replication for your registry\. After you configure replication for your registry, an service\-linked role is created automatically on your behalf\. For more information, see [Private registry settings](registry-settings.md)\. />\.

A service\-linked role makes setting up replication with Amazon ECR easier\. This is because, by using it, you don’t have to manually add all the necessary permissions\. Amazon ECR defines the permissions of its service\-linked roles, and unless defined otherwise, only Amazon ECR can assume its roles\. The defined permissions include the trust policy and the permissions policy,\. The permissions policy can't be attached to any other IAM entity\.

You can delete the service\-linked role only after disabling replication on your registry\. This ensures that you don't inadvertently remove permission for Amazon ECR to replicate your images\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)\. On this linked\-to page, look for the services that have **Yes **in the **Service\-linked role** column\. Choose a **Yes** with a link to view the relevant service\-linked role documentation for that service\.

## Service\-linked role permissions for Amazon ECR<a name="slr-permissions"></a>

Amazon ECR uses the service\-linked role named **AWSServiceRoleForECRReplication** – Allows Amazon ECR to replicate images across multiple accounts\.\.

The AWSServiceRoleForECRReplication service\-linked role trusts the following services to assume the role:
+ `replication.ecr.amazonaws.com`

The role permissions policy allows Amazon ECR to use the following actions on resources:

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

## Supported Regions for Amazon ECR service\-linked roles<a name="slr-regions"></a>

Amazon ECR supports using service\-linked roles in all of the Regions where the service is available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.