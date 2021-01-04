# Private image replication<a name="replication"></a>

Amazon ECR uses **registry settings** to configure private image replication at the registry level\. An Amazon ECR private registry can be configured for either cross\-Region or cross\-account replication\. Replication is configured for a private registry separately for each Region\. The following describes the supported replication methods in more detail\.

**Cross\-Region replication**  
Enabling cross\-Region replication for your registry makes copies of the repositories in one or more destination Regions\. Only images pushed to a repository after cross\-Region replication is configured are copied\.

**Cross\-account replication**  
Enabling cross\-account replication for your registry makes copies of the repositories in the destination account and Regions you specify\. For cross\-account replication to occur, the destination account must configure a registry permissions policy to allow replication from your registry to occur\. For more information, see [Private registry permissions](registry-permissions.md)\.

**Topics**
+ [Considerations for private image replication](#replication-considerations)
+ [Configuring private image replication](registry-settings-configure.md)
+ [Private image replication examples](registry-settings-examples.md)

## Considerations for private image replication<a name="replication-considerations"></a>

The following should be considered when using private image replication\.
+ The first time you configure your private registry for replication, Amazon ECR creates a service\-linked role on your behalf\. The service\-linked role grants the Amazon ECR replication service the permission it needs to create repositories and replicate images in your registry\. For more information, see [Using service\-linked roles for Amazon ECR](using-service-linked-roles.md)\.
+ For cross\-account replication to occur, the destination private registry must grant permission to allow the source registry to replicate its images\. For more information, see [Private registry permissions](registry-permissions.md)\.
+ If the permissions for a registry are changed to remove a permission, any in\-progress replications previously granted may complete\.
+ A replication action only occurs once per image push\. For example, if you configured cross\-Region replication from `us-west-2` to `us-east-1` and from `us-east-1` to `us-east-2`, an image pushed to `us-west-2` replicates to only `us-east-1`, it doesn't replicate again to `us-east-2`\. This behavior applies to both cross\-Region and cross\-account replication\.
+ Registry replication doesn't perform any delete actions\. Replicated images and repositories can be manually deleted when they are no longer being used\.
+ Lifecycle policies aren't replicated and don't have any effect other than the repository they are defined for\. 
+ Repository settings aren't replicated\. The tag immutability, image scanning, and KMS encryption settings are disabled by default on all repositories created because of a replication action\. The tag immutability and image scanning setting can be changed after the repository is created\. However, the setting only applies to images pushed after the setting has changed\.
+ If tag immutability is enabled on a repository and an image is replicated that uses the same tag as an existing image, the image is replicated but won't contain the duplicated tag\. This might result in the image being untagged\.