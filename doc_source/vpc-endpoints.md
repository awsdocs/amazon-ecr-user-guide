# Amazon ECR Interface VPC Endpoints \(AWS PrivateLink\)<a name="vpc-endpoints"></a>

You can improve the security posture of your VPC by configuring Amazon ECR to use an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon ECR APIs by using private IP addresses\. PrivateLink restricts all network traffic between your VPC and Amazon ECR to the Amazon network\. Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. For Amazon ECS tasks using the Fargate launch type, this enables the task to pull private images from Amazon ECR without the need to assign a public IP address to the task\.

For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html#what-is-privatelink)\.

**Topics**
+ [Considerations for Amazon ECR VPC Endpoints](#ecr-vpc-endpoint-considerations)
+ [Creating the VPC Endpoint for Amazon ECR](#ecr-setting-up-vpc-create)
+ [Creating the Amazon S3 Gateway Endpoint](#ecr-setting-up-s3-gateway)
+ [Minimum Amazon S3 Bucket Permissions for Amazon ECR](ecr-minimum-s3-perms.md)

## Considerations for Amazon ECR VPC Endpoints<a name="ecr-vpc-endpoint-considerations"></a>

Before you configure VPC endpoints for Amazon ECR, be aware of the following considerations:
+ To allow your Amazon ECS tasks that use the EC2 launch type to pull private images from Amazon ECR, ensure that you also create the interface VPC endpoints for Amazon ECS\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/vpc-endpoints.html) in the *Amazon Elastic Container Service Developer Guide*\.
**Important**  
Amazon ECS tasks that use the Fargate launch type don't require the Amazon ECS interface VPC endpoints\.
+ Tasks using the Fargate launch type only require the **com\.amazonaws\.*region*\.ecr\.dkr** Amazon ECR VPC endpoint and the Amazon S3 gateway endpoint to take advantage of this feature\.
+ VPC endpoints currently don't support cross\-Region requests\. Ensure that you create your endpoint in the same Region where you plan to issue your API calls to Amazon ECR\.
+ VPC endpoints only support Amazon\-provided DNS through Amazon Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\.
+ The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the VPC\.
+ When you create the Amazon S3 gateway endpoint, if your containers have existing connections to Amazon S3, their connections might be briefly interrupted while the gateway is being added\. If you want to avoid this interruption, create a new VPC that uses the Amazon S3 gateway endpoint and then migrate your Amazon ECS cluster and its containers into the new VPC\.

## Creating the VPC Endpoint for Amazon ECR<a name="ecr-setting-up-vpc-create"></a>

To create the VPC endpoint for the Amazon ECR service, use the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure in the *Amazon VPC User Guide* to create the endpoints described here\.

If your Amazon ECS tasks are using the EC2 launch type, both endpoints are required, and the order that they are created in doesn't matter\. If your tasks are using the Fargate launch type, only the **com\.amazonaws\.*region*\.ecr\.dkr** endpoint is required\.

**com\.amazonaws\.*region*\.ecr\.api**  
*region* represents the Region identifier for an AWS Region supported by Amazon ECR, such as `us-east-2` for the US East \(Ohio\) Region\.
When the `com.amazonaws.region.ecr.api` endpoint is created, enabling a private DNS hostname is optional\. Enabling a private DNS hostname is done by ensuring that the **Enable Private DNS Name** option is selected in the VPC console when you create the VPC endpoint\. If you enable a private DNS hostname for the VPC endpoint, update your SDK or AWS CLI to the latest version so that specifying an endpoint URL when using the SDK or AWS CLI isn't necessary\.  
If you enable a private DNS hostname and are using an SDK or AWS CLI version before January 24, 2019, you must use the `--endpoint-url` parameter to specify the interface endpoints\. The following example CLI command shows the format of the endpoint URL:  

```
aws ecr create-repository --repository-name name --endpoint-url https://api.ecr.region.amazonaws.com
```
If you don't enable a private DNS hostname for the VPC endpoint, you must use the `--endpoint-url` parameter specifying the VPC endpoint ID for the interface endpoint\. The following is the format:  

```
aws ecr create-repository --repository-name name --endpoint-url https://VPC_endpoint_ID.api.ecr.region.vpce.amazonaws.com
```

**com\.amazonaws\.*region*\.ecr\.dkr**  
When you create the `com.amazonaws.region.ecr.dkr` endpoint, you must enable a private DNS hostname\. To do this, ensure that the **Enable Private DNS Name** option is selected in the VPC console when you create the VPC endpoint\.

## Creating the Amazon S3 Gateway Endpoint<a name="ecr-setting-up-s3-gateway"></a>

A gateway endpoint for Amazon S3 is required for all Amazon ECS tasks to pull private images from Amazon ECR because Amazon ECR uses Amazon S3 to store Docker image layers\. When your containers download Docker images from Amazon ECR, they must access Amazon ECR to get the image manifest and Amazon S3 to download the actual image layers\. The following is the Amazon Resource Name \(ARN\) of the Amazon S3 bucket containing the layers for each Docker image:

```
arn:aws:s3:::prod-region-starport-layer-bucket/*
```

To create the Amazon S3 gateway endpoint for the Amazon ECR service, use the [Creating a Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint) procedure in the *Amazon VPC User Guide* to create the following endpoint\. Ensure that when creating the endpoint, you select the route tables for your VPC\.

**com\.amazonaws\.*region*\.s3**  
The Amazon S3 gateway endpoint uses an IAM policy document to limit access to the service\. The **Full Access** policy can be used because any restrictions that you have put in your task IAM roles or other IAM user policies still apply on top of this policy\. If you want to limit Amazon S3 bucket access to the minimum required permissions required to use Amazon ECR, see [Minimum Amazon S3 Bucket Permissions for Amazon ECR](ecr-minimum-s3-perms.md)\.