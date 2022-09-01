# Using service\-linked roles for Amazon ECR<a name="using-service-linked-roles"></a>

Amazon Elastic Container Registry \(Amazon ECR\) uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) to provide the permissions necessary to use the replication and pull through cache features\. A service\-linked role is a unique type of IAM role that is linked directly to Amazon ECR\. The service\-linked role is predefined by Amazon ECR\. It includes all of the permissions that the service requires to support the replication and pull through cache features for your private registry\. After you configure replication or pull through cache for your registry, a service\-linked role is created automatically on your behalf\. For more information, see [Private registry settings](registry-settings.md)\.

A service\-linked role makes setting up replication and pull through cache with Amazon ECR easier\. This is because, by using it, you don’t have to manually add all the necessary permissions\. Amazon ECR defines the permissions of its service\-linked roles, and unless defined otherwise, only Amazon ECR can assume its roles\. The defined permissions include the trust policy and the permissions policy\. The permissions policy can't be attached to any other IAM entity\.

You can delete the corresponding service\-linked role only after disabling either replication or pull through cache on your registry\. This ensures that you don't inadvertently remove the permissions Amazon ECR requires for these features\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)\. On this linked\-to page, look for the services that have **Yes **in the **Service\-linked role** column\. Choose a **Yes** with a link to view the relevant service\-linked role documentation for that service\.

**Topics**
+ [Supported Regions for Amazon ECR service\-linked roles](#slr-regions)
+ [Amazon ECR service\-linked role for replication](slr-replication.md)
+ [Amazon ECR service\-linked role for pull through cache](slr-pullthroughcache.md)

## Supported Regions for Amazon ECR service\-linked roles<a name="slr-regions"></a>

Amazon ECR supports using service\-linked roles in all of the Regions where the Amazon ECR service is available\. For more information about Amazon ECR Region availability, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.