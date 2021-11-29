# Amazon ECR service\-linked role for pull through cache<a name="slr-pullthroughcache"></a>

Amazon ECR uses a service\-linked role named **AWSServiceRoleForECRPullThroughCache** which gives permission for Amazon ECR to push images to your repositories through the pull through cache workflow\.

## Service\-linked role permissions for Amazon ECR<a name="slr-pullthroughcache-permissions"></a>

The **AWSServiceRoleForECRPullThroughCache** service\-linked role trusts the following service to assume the role\.
+ `pullthroughcache.ecr.amazonaws.com`

The following `AWSECRPullThroughCache_ServiceRolePolicy` permissions policy is attached to the service\-linked role and allows Amazon ECR to use the following actions\.

```
{
	"Version": "2012-10-17",
	"Statement": [{
		"Effect": "Allow",
		"Action": [
			"ecr:GetAuthorizationToken",
			"ecr:BatchCheckLayerAvailability",
			"ecr:InitiateLayerUpload",
			"ecr:UploadLayerPart",
			"ecr:CompleteLayerUpload",
			"ecr:PutImage"
		],
		"Resource": "*"
	}]
}
```

You must configure permissions to allow an IAM entity \(for example a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Amazon ECR<a name="slr-pullthroughcache-create"></a>

You don't need to manually create the Amazon ECR service\-linked role for pull through cache\. When you create a pull through cache rule for your private registry in the AWS Management Console, the AWS CLI, or the AWS API, Amazon ECR creates the service\-linked role for you\. 

If you delete this service\-linked role and need to create it again, you can use the same process to recreate the role in your account\. When you create a pull through cache rule for your private registry, Amazon ECR creates the service\-linked role for you again if it doesn't already exist\.

## Editing a service\-linked role for Amazon ECR<a name="slr-pullthroughcache-edit"></a>

Amazon ECR doesn't allow manually editing the **AWSServiceRoleForECRPullThroughCache** service\-linked role\. After the service\-linked role is created, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the service\-linked role for Amazon ECR<a name="slr-pullthroughcache-delete"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way, you don’t have an unused entity that isn't actively monitored or maintained\. However, you must delete the pull through cache rules for your registry in every Region before you can manually delete the service\-linked role\.

**Note**  
If you try to delete resources while the Amazon ECR service is still using the role, your delete action might fail\. If that happens, wait for a few minutes and try again\.

**To delete Amazon ECR resources used by the **AWSServiceRoleForECRPullThroughCache** service\-linked role**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, choose the Region where your pull through cache rules are created\.

1. In the navigation pane, choose **Private registry**, **Pull through cache**\.

1. Select your pull through cache rules and then choose **Delete rule**\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the **AWSServiceRoleForECRPullThroughCache** service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.