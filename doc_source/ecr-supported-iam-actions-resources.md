# Supported Resource\-Level Permissions for Amazon ECR API Actions<a name="ecr-supported-iam-actions-resources"></a>

*Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECR has partial support for resource\-level permissions\. This means that for certain Amazon ECR operations, you can control when users are allowed to use those operations\. Their access can be based on conditions that have to be fulfilled, or specific resources\.

The following table describes the Amazon ECR API operations that currently support resource\-level permissions, as well as the supported resources and resource ARNs for each\.

For a list of Amazon ECR operations, see [Actions](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.

**Important**  
For the Amazon ECR API operations that do not support resource\-level permissions, you can grant users permission to use the operation\. You have to specify the \* \(asterisk\) wildcard for the resource element of your policy statement\.


| API action | Resource Types | Condition Keys | 
| --- | --- | --- | 
|  `BatchCheckLayerAvailability`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `BatchDeleteImage`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `BatchGetImage`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `CompleteLayerUpload`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `CreateRepository`  |  N/A  |  aws:RequestTag  | 
|  `DeleteLifecyclePolicy`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `DeleteRepository`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `DeleteRepositoryPolicy`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `DescribeImages`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `DescribeRepositories`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `GetAuthorizationToken`  |  N/A  |  | 
|  `GetDownloadUrlForLayer`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `GetLifecyclePolicy`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `GetLifecyclePolicyPreview`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `GetRepositoryPolicy`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `InitiateLayerUpload`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `ListImages`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `ListTagsForResource`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `PutImage`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `PutLifecyclePolicy`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `SetRepositoryPolicy`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `StartLifecyclePolicyPreview`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `TagResource`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:RequestTag aws:ResourceTag ecr:ResourceTag  | 
|  `UntagResource`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 
|  `UploadLayerPart`  |  Repository `arn:aws:ecr:region:account:repository/my-repo`  |  aws:ResourceTag ecr:ResourceTag  | 