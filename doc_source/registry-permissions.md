# Private registry permissions<a name="registry-permissions"></a>

Amazon ECR uses a **registry policy** to grant permissions to an AWS principal at the private registry level\. These permissions are used to scope access to the replication and pull through cache features\.

Amazon ECR only enforces the following permissions at the private registry level\. If any additional actions are added to the registry policy, an error will occur\.
+ `ecr:ReplicateImage` – Grants permission to another account, referred to as the source registry, to replicate its images to your registry\. This is only used for cross\-account replication\.
+ `ecr:BatchImportUpstreamImage` – Grants permission to retrieve the external image and import it to your private registry\.
+ `ecr:CreateRepository` – Grants permission to create a repository in a private registry\. This permission is required if the repository storing either the replicated or cached images doesn't already exist in the private registry\.

**Note**  
While it is possible to add the `ecr:*` action to a private registry permissions policy, it is considered best practice to only add the specific actions required based on the feature you're using rather than use a wildcard\.

**Topics**
+ [Setting a private registry permission statement](registry-permissions-create.md)
+ [Deleting a private registry permission statement](registry-permissions-delete.md)
+ [Private registry policy examples](registry-permissions-examples.md)