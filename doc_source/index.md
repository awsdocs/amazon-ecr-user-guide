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
+ [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md)
+ [Getting started with Amazon ECR using the AWS Management Console](getting-started-console.md)
+ [Getting started with Amazon ECR using the AWS CLI](getting-started-cli.md)
+ [Amazon ECR registries](Registries.md)
+ [Amazon ECR repositories](Repositories.md)
   + [Creating a repository](repository-create.md)
   + [Viewing repository information](repository-info.md)
   + [Editing a repository](repository-edit.md)
   + [Deleting a repository](repository-delete.md)
   + [Repository policies](repository-policies.md)
      + [Setting a repository policy statement](set-repository-policy.md)
      + [Deleting a repository policy statement](delete-repository-policy.md)
      + [Repository policy examples](repository-policy-examples.md)
   + [Tagging an Amazon ECR repository](ecr-using-tags.md)
+ [Images](images.md)
   + [Pushing an image](docker-push-ecr-image.md)
   + [Pushing a multi-architecture image](docker-push-multi-architecture-image.md)
   + [Pushing a Helm chart](push-oci-artifact.md)
   + [Pulling an image](docker-pull-ecr-image.md)
   + [Deleting an image](delete_image.md)
   + [Retagging an image](image-retag.md)
   + [Lifecycle policies](LifecyclePolicies.md)
      + [Creating a lifecycle policy preview](lpp_creation.md)
      + [Creating a lifecycle policy](lp_creation.md)
      + [Examples of lifecycle policies](lifecycle_policy_examples.md)
   + [Image tag mutability](image-tag-mutability.md)
   + [Image scanning](image-scanning.md)
   + [Container image manifest formats](image-manifest-formats.md)
   + [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md)
   + [Using Amazon ECR Images with Amazon EKS](ECR_on_EKS.md)
   + [Amazon Linux container image](amazon_linux_container_image.md)
+ [Security in Amazon Elastic Container Registry](security.md)
   + [Identity and Access Management for Amazon Elastic Container Registry](security-iam.md)
      + [How Amazon Elastic Container Registry Works with IAM](security_iam_service-with-iam.md)
      + [Amazon ECR Managed Policies](ecr_managed_policies.md)
      + [Amazon Elastic Container Registry Identity-Based Policy Examples](security_iam_id-based-policy-examples.md)
      + [Using Tag-Based Access Control](ecr-supported-iam-actions-tagging.md)
      + [Troubleshooting Amazon Elastic Container Registry Identity and Access](security_iam_troubleshoot.md)
   + [Data protection in Amazon ECR](data-protection.md)
      + [Encryption at rest](encryption-at-rest.md)
   + [Compliance Validation for Amazon Elastic Container Registry](ecr-compliance.md)
   + [Infrastructure Security in Amazon Elastic Container Registry](infrastructure-security.md)
      + [Amazon ECR interface VPC endpoints (AWS PrivateLink)](vpc-endpoints.md)
+ [Amazon ECR monitoring](monitoring.md)
   + [Visualizing your service quotas and setting alarms](monitoring-quotas-alarms.md)
   + [Amazon ECR usage metrics](monitoring-usage.md)
   + [Amazon ECR usage reports](usage-reports.md)
   + [Amazon ECR events and EventBridge](ecr-eventbridge.md)
   + [Logging Amazon ECR actions with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Amazon ECR service quotas](service-quotas.md)
+ [Amazon ECR Troubleshooting](troubleshooting.md)
   + [Troubleshooting Errors with Docker Commands When Using Amazon ECR](common-errors-docker.md)
   + [Troubleshooting Amazon ECR Error Messages](common-errors.md)
   + [Troubleshooting Image Scanning Issues](image-scanning-troubleshooting.md)
+ [Document History](doc-history.md)
+ [AWS glossary](glossary.md)