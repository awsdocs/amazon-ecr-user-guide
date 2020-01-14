# Amazon ECR User Guide

-----
*****Copyright &copy; 2020 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is Amazon Elastic Container Registry?](what-is-ecr.md)
+ [Setting Up with Amazon ECR](get-set-up-for-amazon-ecr.md)
+ [Docker Basics for Amazon ECR](docker-basics.md)
+ [Getting Started with Amazon ECR](ECR_GetStarted.md)
+ [Amazon ECR Registries](Registries.md)
+ [Amazon ECR Repositories](Repositories.md)
   + [Creating a Repository](repository-create.md)
   + [Viewing Repository Information](repository-info.md)
   + [Editing a Repository](repository-edit.md)
   + [Deleting a Repository](repository-delete.md)
   + [Amazon ECR Repository Policies](repository-policies.md)
      + [Setting a Repository Policy Statement](set-repository-policy.md)
      + [Deleting a Repository Policy Statement](delete-repository-policy.md)
      + [Amazon ECR Repository Policy Examples](repository-policy-examples.md)
   + [Tagging an Amazon ECR Repository](ecr-using-tags.md)
+ [Images](images.md)
   + [Pushing an Image](docker-push-ecr-image.md)
   + [Pulling an Image](docker-pull-ecr-image.md)
   + [Deleting an Image](delete_image.md)
   + [Retagging an Image](image-retag.md)
   + [Amazon ECR Lifecycle Policies](LifecyclePolicies.md)
      + [Creating a Lifecycle Policy Preview](lpp_creation.md)
      + [Creating a Lifecycle Policy](lp_creation.md)
      + [Examples of Lifecycle Policies](lifecycle_policy_examples.md)
   + [Image Tag Mutability](image-tag-mutability.md)
   + [Image Scanning](image-scanning.md)
   + [Container Image Manifest Formats](image-manifest-formats.md)
   + [Using Amazon ECR Images with Amazon ECS](ECR_on_ECS.md)
   + [Using Amazon ECR Images with Amazon EKS](ECR_on_EKS.md)
   + [Amazon Linux Container Image](amazon_linux_container_image.md)
+ [Security in Amazon Elastic Container Registry](security.md)
   + [Identity and Access Management for Amazon Elastic Container Registry](security-iam.md)
      + [How Amazon Elastic Container Registry Works with IAM](security_iam_service-with-iam.md)
      + [Amazon ECR Managed Policies](ecr_managed_policies.md)
      + [Amazon Elastic Container Registry Identity-Based Policy Examples](security_iam_id-based-policy-examples.md)
      + [Using Tag-Based Access Control](ecr-supported-iam-actions-tagging.md)
      + [Troubleshooting Amazon Elastic Container Registry Identity and Access](security_iam_troubleshoot.md)
   + [Compliance Validation for Amazon Elastic Container Registry](ecr-compliance.md)
   + [Infrastructure Security in Amazon Elastic Container Registry](infrastructure-security.md)
      + [Amazon ECR Interface VPC Endpoints (AWS PrivateLink)](vpc-endpoints.md)
         + [Minimum Amazon S3 Bucket Permissions for Amazon ECR](ecr-minimum-s3-perms.md)
+ [Amazon ECR Events and EventBridge](ecr-eventbridge.md)
+ [Using the AWS CLI with Amazon ECR](ECR_AWSCLI.md)
+ [Amazon ECR Service Quotas](service-quotas.md)
+ [Amazon ECR Usage Reports](usage-reports.md)
+ [Logging Amazon ECR API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Amazon ECR Troubleshooting](troubleshooting.md)
   + [Troubleshooting Errors with Docker Commands When Using Amazon ECR](common-errors-docker.md)
   + [Troubleshooting Amazon ECR Error Messages](common-errors.md)
   + [Troubleshooting Image Scanning Issues](image-scanning-troubleshooting.md)
+ [Document History](doc-history.md)
+ [AWS Glossary](glossary.md)