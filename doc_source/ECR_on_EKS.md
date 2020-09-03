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

## Installing a Helm chart hosted on Amazon ECR with Amazon EKS<a name="using-helm-charts-eks"></a>

Your Helm charts hosted in Amazon ECR can be installed on your Amazon EKS clusters\. The following steps demonstrate this\.

**Prerequisites**  
Before you begin, ensure the following steps have been completed\.
+ Install the Helm client version 3\. For more information, see [Installing Helm](https://helm.sh/docs/intro/install/)\.
+ You have pushed a Helm chart to your Amazon ECR repository\. For more information, see [Pushing a Helm chart](push-oci-artifact.md)\.
+ You have configured `kubectl` to work with Amazon EKS\. For more information, see [Create a `kubeconfig` for Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html) in the **Amazon EKS User Guide**\. If the following commands succeeds for your cluster, you're properly configured\.

  ```
  kubectl get svc
  ```

**Install an Amazon ECR hosted Helm chart to an Amazon EKS cluster**

1. Enable OCI support in the Helm 3 client\.

   ```
   export HELM_EXPERIMENTAL_OCI=1
   ```

1. Authenticate your Helm client to the Amazon ECR registry that your Helm chart is hosted\. Authentication tokens must be obtained for each registry used, and the tokens are valid for 12 hours\. For more information, see [Registry authentication](Registries.md#registry_auth)\.

   ```
   aws ecr get-login-password \
        --region us-west-2 | helm registry login \
        --username AWS \
        --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
   ```

1. Pull your Helm chart to your local cache\.

   ```
   helm chart pull aws_account_id.dkr.ecr.region.amazonaws.com/repository-name:mychart
   ```

1. Export the chart to a local directory\. In this example, we use a directory named `charts`\.

   ```
   helm chart export aws_account_id.dkr.ecr.region.amazonaws.com/repository-name:mychart --destination ./charts
   ```

1. Install the chart\.

   ```
   helm install ecr-chart-demo ./mychart
   ```

   The output should look similar to this:

   ```
   NAME: ecr-chart-demo
   LAST DEPLOYED: Wed Sep  2 14:32:07 2020
   NAMESPACE: default
   STATUS: deployed
   REVISION: 1
   NOTES:
   ```

1. Verify the chart installation\. The output will be a YAML representation of the Kubernetes resources deployed by the chart\.

   ```
   helm get manifest ecr-chart-demo
   ```

1. When you are finished, you can remove the chart release from your cluster\.

   ```
   helm uninstall ecr-chart-demo
   ```