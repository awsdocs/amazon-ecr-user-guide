# Private image replication<a name="replication"></a>

Amazon ECR uses **registry settings** to configure private image replication at the registry level\. An Amazon ECR private registry can be configured for either cross\-Region or cross\-account replication\. Replication is configured for a private registry separately for each Region\. The following describes the supported replication methods in more detail\.

**Cross\-Region replication**  
Enabling cross\-Region replication for your registry makes copies of the repositories in one or more destination Regions\.

**Cross\-account replication**  
Enabling cross\-account replication for your registry makes copies of the repositories in the destination account and Regions you specify\. For cross\-account replication to occur, the destination account must configure a registry permissions policy to allow replication from your registry to occur\. For more information, see [Private registry permissions](registry-permissions.md)\.

**Topics**
+ [Considerations for private image replication](#replication-considerations)
+ [Configuring private image replication](registry-settings-configure.md)
+ [Viewing replication status](replication-status.md)
+ [Private image replication examples](registry-settings-examples.md)

## Considerations for private image replication<a name="replication-considerations"></a>

The following should be considered when using private image replication\.
+ Only images pushed to a repository after replication is configured are copied\.
+ The first time you configure your private registry for replication, Amazon ECR creates a service\-linked IAM role on your behalf\. The service\-linked IAM role grants the Amazon ECR replication service the permission it needs to create repositories and replicate images in your registry\. For more information, see [Using service\-linked roles for Amazon ECR](using-service-linked-roles.md)\.
+ For cross\-account replication to occur, the private registry destination must grant permission to allow the source registry to replicate its images\. This is done by setting a private registry permissions policy\. For more information, see [Private registry permissions](registry-permissions.md)\.
+ If the permission policy for a private registry are changed to remove a permission, any in\-progress replications previously granted may complete\.
+ A Region must be enabled for an account prior to any replication actions occurring within or to that Region\. For more information, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *Amazon Web Services General Reference*\.
+ Cross\-Region replication is not supported between AWS partitions\. For example, a repository in `us-west-2` can't be replicated to `cn-north-1`\. For more information about AWS partitions, see [ARN format](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arns-syntax) in the *AWS General Reference*\.
+ The replication configuration for a private registry may contain up to 25 unique destinations across all rules, with a maximum of 10 rules total\. Each rule may contain up to 100 filters\. This allows for specifying separate rules for repositories containing images used for production and testing, for example\.
+ The replication configuration supports filtering which repositories in a private registry are replicated by specifying a repository prefix\. For an example, see [Example: Configuring cross\-Region replication using a repository filter](registry-settings-examples.md#registry-settings-examples-crr-filter)\.
+ A replication action only occurs once per image push\. For example, if you configured cross\-Region replication from `us-west-2` to `us-east-1` and from `us-east-1` to `us-east-2`, an image pushed to `us-west-2` replicates to only `us-east-1`, it doesn't replicate again to `us-east-2`\. This behavior applies to both cross\-Region and cross\-account replication\.
+ The majority of images replicate in less than 30 minutes, but in rare cases the replication might take longer\.
+ Registry replication doesn't perform any delete actions\. Replicated images and repositories can be manually deleted when they are no longer being used\.
+ Repository policies, including IAM policies, and lifecycle policies aren't replicated and don't have any effect other than on the repository they are defined for\.
+ Repository settings aren't replicated\. The tag immutability, image scanning, and KMS encryption settings are disabled by default on all repositories created because of a replication action\. The tag immutability and image scanning setting can be changed after the repository is created\. However, the setting only applies to images pushed after the setting has changed\.
+ If tag immutability is enabled on a repository and an image is replicated that uses the same tag as an existing image, the image is replicated but won't contain the duplicated tag\. This might result in the image being untagged\.