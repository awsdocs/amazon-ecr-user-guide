# Private registry permissions<a name="registry-permissions"></a>

Amazon ECR uses a **registry policy** to grant permissions to an AWS principal, allowing the replication of the repositories from a source registry to your registry\. By default, you have permission to configure cross\-Region replication within your own registry\. You only need to configure the registry policy if you're granting another account permission to replicate contents to your registry\.

A registry policy must grant permission for the `ecr:ReplicateImage` API action\. This API is an internal Amazon ECR API that can replicate images between Regions or accounts\. You can also grant permission for the `ecr:CreateRepository` permission, which allows Amazon ECR to create repositories in your registry if they don't exist already\. If the `ecr:CreateRepository` permission isn't provided, a repository with the same name as the source repository must be created manually in your registry\. If neither is done, replication fails\. Any failed CreateRepository or ReplicateImage API actions show up in CloudTrail\.

**Topics**
+ [Setting a private registry permission statement](registry-permissions-create.md)
+ [Deleting a private registry permission statement](registry-permissions-delete.md)
+ [Private registry policy examples](registry-permissions-examples.md)