# Using Amazon ECR images with Amazon ECS<a name="ECR_on_ECS"></a>

You can use your container images hosted in Amazon ECR in your Amazon ECS task definitions, but you need to satisfy the following prerequisites\.
+ When using the EC2 launch type for your Amazon ECS tasks, your container instances must be using at least version 1\.7\.0 of the Amazon ECS container agent\. The latest version of the Amazon ECS–optimized AMI supports Amazon ECR images in task definitions\. For more information, including the latest Amazon ECS–optimized AMI IDs, see [Amazon ECS\-optimized AMI versions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-ami-versions.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ The Amazon ECS container instance IAM role \(`ecsInstanceRole`\) that you use must contain the following IAM policy permissions for Amazon ECR\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "ecr:BatchCheckLayerAvailability",
                  "ecr:BatchGetImage",
                  "ecr:GetDownloadUrlForLayer",
                  "ecr:GetAuthorizationToken"
              ],
              "Resource": "*"
          }
      ]
  }
  ```

  If you use the **AmazonEC2ContainerServiceforEC2Role** managed policy, then your container instance IAM role has the proper permissions\. To check that your role supports Amazon ECR, see [Amazon ECS container instance IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ In your Amazon ECS task definitions, make sure that you are using the full `registry/repository:tag` naming for your Amazon ECR images\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest`\.

  The following task definition snippet shows the syntax you would use to specify a container image hosted in Amazon ECR in your Amazon ECS task definition\.

  ```
  {
      "family": "task-definition-name",
      ...
      "containerDefinitions": [
          {
              "name": "container-name",
              "image": "aws_account_id.dkr.ecr.region.amazonaws.com/my-web-app:latest",
              ...
          }
      ],
      ...
  }
  ```