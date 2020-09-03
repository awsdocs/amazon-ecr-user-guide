# What Is Amazon Elastic Container Registry?<a name="what-is-ecr"></a>

Amazon Elastic Container Registry \(Amazon ECR\) is a managed AWS container image registry service that is secure, scalable, and reliable\. Amazon ECR supports private container image repositories with resource\-based permissions using AWS IAM so that specific users or Amazon EC2 instances can access repositories and images\. Developers can use the their preferred CLI to push, pull, and manage Docker images, Open Container Initiative \(OCI\) images, and OCI compatible artifacts\.

The AWS container services team maintains a public roadmap on GitHub\. It contains information about what the teams are working on and allows all AWS customers the ability to give direct feedback\. For more information, see [AWS Containers Roadmap](https://github.com/aws/containers-roadmap)\.

## Components of Amazon ECR<a name="ecr-components"></a>

Amazon ECR contains the following components:

Registry  
An Amazon ECR registry is provided to each AWS account; you can create image repositories in your registry and store images in them\. For more information, see [Amazon ECR registries](Registries.md)\.

Authorization token  
Your client must authenticate to Amazon ECR registries as an AWS user before it can push and pull images\. For more information, see [Registry authentication](Registries.md#registry_auth)\.

Repository  
An Amazon ECR image repository contains your Docker images, Open Container Initiative \(OCI\) images, and OCI compatible artifacts\. For more information, see [Amazon ECR repositories](Repositories.md)\.

Repository policy  
You can control access to your repositories and the images within them with repository policies\. For more information, see [Repository policies](repository-policies.md)\.

Image  
You can push and pull container images to your repositories\. You can use these images locally on your development system, or you can use them in Amazon ECS task definitions and Amazon EKS pod specifications\. For more information, see [Using Amazon ECR images with Amazon ECS](ECR_on_ECS.md) and [Using Amazon ECR Images with Amazon EKS](ECR_on_EKS.md)\.

## Features of Amazon ECR<a name="ecr-features"></a>

Amazon ECR provides the following features:
+ Lifecycle policies enable you to manage the lifecycle of the images in your repositories\. You define rules which result in the cleaning up of unused images which you can test before applying to your repository\. For more information, see [Lifecycle policies](LifecyclePolicies.md)\.
+ Image scanning helps in identifying software vulnerabilities in your container images\. Each repository can be configured to **scan on push**, which ensures that each new image pushed to the repository is scanned\. You can then retrieve the results of the image scan\. For more information, see [Image scanning](image-scanning.md)\.

## How to Get Started with Amazon ECR<a name="ecr-get-started"></a>

To use Amazon ECR, you must be set up to install the AWS Command Line Interface and Docker\. For more information, see [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md) and [Getting started with Amazon ECR using the AWS CLI](getting-started-cli.md)\.

## Pricing for Amazon ECR<a name="ecr-pricing"></a>

With Amazon ECR, you only pay for the amount of data you store in your repositories and for the data transfer from your image pushes and pulls\. For more information, see [Amazon ECR pricing](http://aws.amazon.com/ecr/pricing/)\.