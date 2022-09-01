# What is Amazon Elastic Container Registry?<a name="what-is-ecr"></a>

Amazon Elastic Container Registry \(Amazon ECR\) is an AWS managed container image registry service that is secure, scalable, and reliable\. Amazon ECR supports private repositories with resource\-based permissions using AWS IAM\. This is so that specified users or Amazon EC2 instances can access your container repositories and images\. You can use your preferred CLI to push, pull, and manage Docker images, Open Container Initiative \(OCI\) images, and OCI compatible artifacts\.

**Note**  
Amazon ECR supports public container image repositories as well\. For more information, see [What is Amazon ECR Public](https://docs.aws.amazon.com/AmazonECR/latest/public/what-is-ecr.html) in the *Amazon ECR Public User Guide*\.

The AWS container services team maintains a public roadmap on GitHub\. It contains information about what the teams are working on and allows all AWS customers the ability to give direct feedback\. For more information, see [AWS Containers Roadmap](https://github.com/aws/containers-roadmap)\.

## Components of Amazon ECR<a name="ecr-components"></a>

Amazon ECR contains the following components:

Registry  
An Amazon ECR private registry is provided to each AWS account; you can create one or more repositories in your registry and store images in them\. For more information, see [Amazon ECR private registry](Registries.md)\.

Authorization token  
Your client must authenticate to Amazon ECR registries as an AWS user before it can push and pull images\. For more information, see [Private registry authentication](registry_auth.md)\.

Repository  
An Amazon ECR repository contains your Docker images, Open Container Initiative \(OCI\) images, and OCI compatible artifacts\. For more information, see [Amazon ECR private repositories](Repositories.md)\.

Repository policy  
You can control access to your repositories and the images within them with repository policies\. For more information, see [Private repository policies](repository-policies.md)\.

Image  
You can push and pull container images to your repositories\. You can use these images locally on your development system, or you can use them in Amazon ECS task definitions and Amazon EKS pod specifications\. For more information, see [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md) and [Using Amazon ECR Images with Amazon EKS](ECR_on_EKS.md)\.

## Features of Amazon ECR<a name="ecr-features"></a>

Amazon ECR provides the following features:
+ Lifecycle policies help with managing the lifecycle of the images in your repositories\. You define rules that result in the cleaning up of unused images\. You can test rules before applying them to your repository\. For more information, see [Lifecycle policies](LifecyclePolicies.md)\.
+ Image scanning helps in identifying software vulnerabilities in your container images\. Each repository can be configured to **scan on push**\. This ensures that each new image pushed to the repository is scanned\. You can then retrieve the results of the image scan\. For more information, see [Image scanning](image-scanning.md)\.
+ Cross\-Region and cross\-account replication makes it easier for you to have your images where you need them\. This is configured as a registry setting and is on a per\-Region basis\. For more information, see [Private registry settings](registry-settings.md)\.
+ Pull through cache rules provide a way to cache repositories in remote public registries in your private Amazon ECR registry\. Using a pull through cache rule, Amazon ECR will periodically reach out to the remote registry to ensure the cached image in your Amazon ECR private registry is up to date\. For more information, see [Using pull through cache rules](pull-through-cache.md)\.

## How to get started with Amazon ECR<a name="ecr-get-started"></a>

To use Amazon ECR, you must be set up to install the AWS Command Line Interface and Docker\. For more information, see [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md) and [Using Amazon ECR with the AWS CLI](getting-started-cli.md)\.

## Pricing for Amazon ECR<a name="ecr-pricing"></a>

With Amazon ECR, you only pay for the amount of data you store in your repositories and for the data transfer from your image pushes and pulls\. For more information, see [Amazon ECR pricing](http://aws.amazon.com/ecr/pricing/)\.