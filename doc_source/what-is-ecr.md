# What Is Amazon Elastic Container Registry?<a name="what-is-ecr"></a>

Amazon Elastic Container Registry \(Amazon ECR\) is a managed AWS Docker registry service that is secure, scalable, and reliable\. Amazon ECR supports private Docker repositories with resource\-based permissions using AWS IAM so that specific users or Amazon EC2 instances can access repositories and images\. Developers can use the Docker CLI to push, pull, and manage images\.

## Components of Amazon ECR<a name="w3ab1b7b7"></a>

Amazon ECR contains the following components:

Registry  
An Amazon ECR registry is provided to each AWS account; you can create image repositories in your registry and store images in them\. For more information, see [Amazon ECR Registries](Registries.md)\.

Authorization token  
Your Docker client needs to authenticate to Amazon ECR registries as an AWS user before it can push and pull images\. The AWS CLI get\-login command provides you with authentication credentials to pass to Docker\. For more information, see [Registry Authentication](Registries.md#registry_auth)\.

Repository  
An Amazon ECR image repository contains your Docker images\. For more information, see [Amazon ECR Repositories](Repositories.md)\.

Repository policy  
You can control access to your repositories and the images within them with repository policies\. For more information, see [Amazon ECR Repository Policies](RepositoryPolicies.md)\.

Image  
You can push and pull Docker images to your repositories\. You can use these images locally on your development system, or you can use them in Amazon ECS task definitions\. For more information, see [Using Amazon ECR Images with Amazon ECS](ECR_on_ECS.md)\.

## How to Get Started with Amazon ECR<a name="w3ab1b7b9"></a>

To use Amazon ECR, you need to be set up to install the AWS Command Line Interface and Docker\. For more information, see [Setting Up with Amazon ECR](get-set-up-for-amazon-ecr.md) and [Docker Basics for Amazon ECR](docker-basics.md)\.

After you are set up, you are ready to complete the [Getting Started with Amazon ECR](ECR_GetStarted.md) tutorial\. 