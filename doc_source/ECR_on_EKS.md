# Using Amazon ECR Images with Amazon EKS<a name="ECR_on_EKS"></a>

You can use your Amazon ECR images with Amazon EKS, but you need to satisfy the following prerequisites\.
+ The Amazon EKS worker node IAM role \(`NodeInstanceRole`\) that you use with your worker nodes must possess the following IAM policy permissions for Amazon ECR\.

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
**Note**  
If you used `eksctl` or the AWS CloudFormation templates in [Getting Started with Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html) to create your cluster and worker node groups, these IAM permissions are applied to your worker node IAM role by default\.
+ When referencing an image from Amazon ECR, you must use the full `registry/repository:tag` naming for the image\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest`\.