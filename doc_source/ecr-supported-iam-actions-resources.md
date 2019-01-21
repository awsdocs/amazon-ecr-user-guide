# Supported Resource\-Level Permissions for Amazon ECR API Actions<a name="ecr-supported-iam-actions-resources"></a>

*Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECR has partial support for resource\-level permissions\. This means that for certain Amazon ECR operations, you can control when users are allowed to use those operations\. Their access can be based on conditions that have to be fulfilled, or specific resources\.

The following table describes the Amazon ECR API operations that currently support resource\-level permissions, as well as the supported resources and resource ARNs for each\.

For a list of Amazon ECR operations, see [Actions](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.

**Important**  
For the Amazon ECR API operations that do not support resource\-level permissions, you can grant users permission to use the operation\. You have to specify the \* \(asterisk\) wildcard for the resource element of your policy statement\.


| API action | Resource Repository | 
| --- | --- | 
| BatchCheckLayerAvailability |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| BatchDeleteImage |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| BatchGetImage |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| CompleteLayerUpload |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| CreateRepository |  This action does not support resource\-level permissions\.  | 
| DeleteLifecyclePolicy |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| DeleteRepository |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| DeleteRepositoryPolicy |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| DescribeImages |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| DescribeRepositories |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| GetAuthorizationToken |  This action does not support resource\-level permissions\.  | 
| GetDownloadUrlForLayer |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| GetLifecyclePolicy |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| GetLifecyclePolicyPreview |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| GetRepositoryPolicy |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| InitiateLayerUpload |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| ListImages |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| PutImage |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| PutLifecyclePolicy |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| SetRepositoryPolicy |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| StartLifecyclePolicyPreview |  `arn:aws:ecr:region:account:repository/my-repo`  | 
| UploadLayerPart |  `arn:aws:ecr:region:account:repository/my-repo`  | 