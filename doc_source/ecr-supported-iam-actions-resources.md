# Supported Resource\-Level Permissions for Amazon ECR API Actions<a name="ecr-supported-iam-actions-resources"></a>

*Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECR has partial support for resource\-level permissions\. This means that for certain Amazon ECR operations, you can control when users are allowed to use those operations based on conditions that have to be fulfilled, or specific resources that users are allowed to use\.

The following table describes the Amazon ECR API operations that currently support resource\-level permissions, as well as the supported resources and resource ARNs for each\.

For a list of Amazon ECR operations, see [Actions](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.


| API action | Resource | 
| --- | --- | 
| BatchCheckLayerAvailability |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| BatchDeleteImage |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| BatchGetImage |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| CompleteLayerUpload |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| CreateRepository |  *  | 
| DeleteLifecyclePolicy |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| DeleteRepository |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| DeleteRepositoryPolicy |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| DescribeImages |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| DescribeRepositories |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| GetAuthorizationToken |  *  | 
| GetDownloadUrlForLayer |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| GetLifecyclePolicy |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| GetLifecyclePolicyPreview |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| GetRepositoryPolicy |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| InitiateLayerUpload |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| ListImages |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| PutImage |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| PutLifecyclePolicy |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| SetRepositoryPolicy |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| StartLifecyclePolicyPreview |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
| UploadLayerPart |  Repository arn:aws:ecr:*region*:*account*:repository/*my\-repo*  | 
