# Amazon ECR Interface VPC Endpoints \(AWS PrivateLink\)<a name="vpc-endpoints"></a>

You can improve the security posture of your VPC by configuring Amazon ECR to use an interface VPC endpoint\. VPC endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon ECR APIs through private IP addresses\. AWS PrivateLink restricts all network traffic between your VPC and Amazon ECR to the Amazon network\. Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. For Amazon ECS tasks using the Fargate launch type, the VPC endpoint enables the task to pull private images from Amazon ECR without assigning a public IP address to the task\.

For more information about AWS PrivateLink and VPC endpoints, see [VPC Endpoints ](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\.

**Topics**
+ [Considerations for Amazon ECR VPC Endpoints](#ecr-vpc-endpoint-considerations)
+ [Create the VPC Endpoint for Amazon ECR](#ecr-setting-up-vpc-create)
+ [Create the Amazon S3 Gateway Endpoint](#ecr-setting-up-s3-gateway)
+ [Create the CloudWatch Logs Endpoint](#ecr-setting-up-cloudwatch-logs)
+ [Create an Endpoint Policy for your Amazon ECR VPC Endpoint](#ecr-vpc-endpoint-policy)
+ [Special Considerations for Windows Images for Amazon ECR](#ecr-vpc-endpoint-windows)

## Considerations for Amazon ECR VPC Endpoints<a name="ecr-vpc-endpoint-considerations"></a>

Before you configure VPC endpoints for Amazon ECR, be aware of the following considerations:
+ To allow your Amazon ECS tasks that use the EC2 launch type to pull private images from Amazon ECR, ensure that you also create the interface VPC endpoints for Amazon ECS\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/vpc-endpoints.html) in the *Amazon Elastic Container Service Developer Guide*\.
**Important**  
Amazon ECS tasks that use the Fargate launch type don't require the Amazon ECS interface VPC endpoints\.
+ Amazon ECS tasks using the Fargate launch type and platform version 1\.3\.0 or earlier only require the **com\.amazonaws\.*region*\.ecr\.dkr** Amazon ECR VPC endpoint and the Amazon S3 gateway endpoint to take advantage of this feature\.
+ Amazon ECS tasks using the Fargate launch type and platform version 1\.4\.0 or later require the **com\.amazonaws\.*region*\.ecr\.dkr** and **com\.amazonaws\.*region*\.ecr\.api ** Amazon ECR VPC endpoints and the Amazon S3 gateway endpoint to take advantage of this feature\.
+ Amazon ECS tasks using the Fargate launch type that pull container images from Amazon ECR can restrict access to the specific VPC their tasks use and to the VPC endpoint the service uses by adding condition keys to their task execution role\. For more information, see [Amazon ECS Task Execution IAM Role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ VPC endpoints currently don't support cross\-Region requests\. Ensure that you create your endpoint in the same Region where you plan to issue your API calls to Amazon ECR\.
+ VPC endpoints only support Amazon provided DNS through Amazon Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\.
+ The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the VPC\.
+ If your containers have existing connections to Amazon S3, their connections might be briefly interrupted when you add the Amazon S3 gateway endpoint\. If you want to avoid this interruption, create a new VPC that uses the Amazon S3 gateway endpoint and then migrate your Amazon ECS cluster and its containers into the new VPC\.

## Create the VPC Endpoint for Amazon ECR<a name="ecr-setting-up-vpc-create"></a>

To create the VPC endpoints for the Amazon ECR service, use the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure in the *Amazon VPC User Guide*\.

If your Amazon ECS tasks use the EC2 launch type, both of the following endpoints are required\. The order that the endpoints are created in doesn't matter\. If your tasks are using the Fargate launch type, only the **com\.amazonaws\.*region*\.ecr\.dkr** endpoint is required\.

**com\.amazonaws\.*region*\.ecr\.api**  
The specified *region* represents the Region identifier for an AWS Region supported by Amazon ECR, such as `us-east-2` for the US East \(Ohio\) Region\.
This endpoint is used for calls to the Amazon ECR API\. API actions such as DescribeImages and CreateRepositories go to this endpoint\.  
When the `com.amazonaws.region.ecr.api` endpoint is created, you have the option to enable a private DNS hostname\. Enable this hostname by selecting **Enable Private DNS Name** in the VPC console when you create the VPC endpoint\. If you enable a private DNS hostname for the VPC endpoint, update your SDK or AWS CLI to the latest version so that specifying an endpoint URL when using the SDK or AWS CLI isn't necessary\.  
If you enable a private DNS hostname and are using an SDK or AWS CLI version released before January 24, 2019, you must use the `--endpoint-url` parameter to specify the interface endpoints\. The following example CLI command shows the format of the endpoint URL\.  

```
aws ecr create-repository --repository-name name --endpoint-url https://api.ecr.region.amazonaws.com
```
If you don't enable a private DNS hostname for the VPC endpoint, you must use the `--endpoint-url` parameter specifying the VPC endpoint ID for the interface endpoint\. Following is the format for the endpoint URL\.  

```
aws ecr create-repository --repository-name name --endpoint-url https://VPC_endpoint_ID.api.ecr.region.vpce.amazonaws.com
```

**com\.amazonaws\.*region*\.ecr\.dkr**  
This endpoint is used for the Docker Registry APIs\. Docker client commands such as `push` and `pull` use this endpoint\.  
When you create the `com.amazonaws.region.ecr.dkr` endpoint, you must enable a private DNS hostname\. To do this, ensure that the **Enable Private DNS Name** option is selected in the VPC console when you create the VPC endpoint\.

## Create the Amazon S3 Gateway Endpoint<a name="ecr-setting-up-s3-gateway"></a>

To pull private images from Amazon ECR, you must create a gateway endpoint for Amazon S3 for all Amazon ECS tasks\. The gateway endpoint is required because Amazon ECR uses Amazon S3 to store Docker image layers\. When your containers download Docker images from Amazon ECR, they must access Amazon ECR to get the image manifest and Amazon S3 to download the actual image layers\. The following is the Amazon Resource Name \(ARN\) of the Amazon S3 bucket containing the layers for each Docker image\.

```
arn:aws:s3:::prod-region-starport-layer-bucket/*
```

Use the [Creating a Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint) procedure in the *Amazon VPC User Guide* to create the following Amazon S3 gateway endpoint for the Amazon ECR service\. When you create the endpoint, be sure to select the route tables for your VPC\.

**com\.amazonaws\.*region*\.s3**  
The Amazon S3 gateway endpoint uses an IAM policy document to limit access to the service\. The **Full Access** policy can be used because any restrictions that you have put in your task IAM roles or other IAM user policies still apply on top of this policy\. If you want to limit Amazon S3 bucket access to the minimum required permissions for using Amazon ECR, see [Minimum Amazon S3 Bucket Permissions for Amazon ECR](#ecr-minimum-s3-perms)\.

### Minimum Amazon S3 Bucket Permissions for Amazon ECR<a name="ecr-minimum-s3-perms"></a>

The Amazon S3 gateway endpoint uses an IAM policy document to limit access to the service\. To allow only the minimum Amazon S3 bucket permissions for Amazon ECR, restrict access to the Amazon S3 bucket that Amazon ECR uses when you create the IAM policy document for the endpoint\. 

The following table describes the Amazon S3 bucket policy permissions needed by Amazon ECR\.


| Permission | Description | 
| --- | --- | 
| arn:aws:s3:::prod\-region\-starport\-layer\-bucket/\* | Provides access to the Amazon S3 bucket containing the layers for each Docker image\. Represents the Region identifier for an AWS Region supported by Amazon ECR, such as us\-east\-2 for the US East \(Ohio\) Region\. | 

#### Example<a name="ecr-minimum-s3-perms-example"></a>

The following example illustrates how to provide access to the Amazon S3 buckets required for Amazon ECR operations\.

```
{
  "Statement": [
    {
      "Sid": "Access-to-specific-bucket-only",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": ["arn:aws:s3:::prod-region-starport-layer-bucket/*"]
    }
  ]
}
```

## Create the CloudWatch Logs Endpoint<a name="ecr-setting-up-cloudwatch-logs"></a>

For tasks using the Fargate launch type, if your VPC doesn't have an internet gateway and your tasks use the `awslogs` log driver to send log information to CloudWatch Logs, you must create the **com\.amazonaws\.*region*\.logs** interface VPC endpoint for CloudWatch Logs\. For more information, see [Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html) in the *Amazon CloudWatch Logs User Guide*\.

## Create an Endpoint Policy for your Amazon ECR VPC Endpoint<a name="ecr-vpc-endpoint-policy"></a>

A VPC endpoint policy is an IAM resource policy that you attach to an endpoint when you create or modify the endpoint\. If you don't attach a policy when you create an endpoint, AWS attaches a default policy for you that allows full access to the service\. An endpoint policy doesn't override or replace IAM user policies or service\-specific policies\. It's a separate policy for controlling access from the endpoint to the specified service\. Endpoint policies must be written in JSON format\. For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

We recommend creating a single IAM resource policy and attaching it to both of the Amazon ECR VPC endpoints\.

The following is an example of an endpoint policy for Amazon ECR\. This policy enables a specific IAM role to pull images from Amazon ECR\.

```
{
	"Statement": [{
		"Sid": "AllowPull",
		"Principal": {
			"AWS": "arn:aws:iam::1234567890:role/role_name"
		},
		"Action": [
			"ecr:BatchGetImage",
			"ecr:GetDownloadUrlForLayer"
		],
		"Effect": "Allow",
		"Resource": "*"
	}]
}
```

The following endpoint policy example prevents a specified repository from being deleted\.

```
{
	"Statement": [{
			"Sid": "AllowAll",
			"Principal": "*",
			"Action": "*",
			"Effect": "Allow",
			"Resource": "*"
		},
		{
			"Sid": "PreventDelete",
			"Principal": "*",
			"Action": "ecr:DeleteRepository",
			"Effect": "Deny",
			"Resource": "arn:aws:ecr:region:1234567890:repository/repository_name"
		}
	]
}
```

The following endpoint policy example combines the two previous examples into a single policy\.

```
{
	"Statement": [{
			"Sid": "AllowAll",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "*",
			"Resource": "*"
		},
		{
			"Sid": "PreventDelete",
			"Effect": "Deny",
			"Principal": "*",
			"Action": "ecr:DeleteRepository",
			"Resource": "arn:aws:ecr:region:1234567890:repository/repository_name"
		},
		{
			"Sid": "AllowPull",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::1234567890:role/role_name"
			},
			"Action": [
				"ecr:BatchGetImage",
				"ecr:GetDownloadUrlForLayer"
			],
			"Resource": "*"
		}
	]
}
```

**To modify the VPC endpoint policy for Amazon ECR**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**\.

1. If you have not already created the VPC endpoints for Amazon ECR, see [Create the VPC Endpoint for Amazon ECR](#ecr-setting-up-vpc-create)\.

1. Select the Amazon ECR VPC endpoint to add a policy to, and choose the **Policy** tab in the lower half of the screen\.

1. Choose **Edit Policy** and make the changes to the policy\.

1. Choose **Save** to save the policy\.

## Special Considerations for Windows Images for Amazon ECR<a name="ecr-vpc-endpoint-windows"></a>

Images for the Windows Operating System type include artifacts whose distribution is restricted by license. When you build on these images and push them to ECR you will notice the base layer is never pushed. These base layers are referenced using the concept of a foreign layer that points to the real base layer residing in Azure infrastructure. For this reason it is not sufficient to enable the required VPC Endpoint infrastructure as your instances may need to pull the foreign layers from Azure.

It is possible to override this behaviour when pushing images to ECR using the `--allow-nondistributable-artifacts` flag in the Docker daemon. This flag, when enabled, will push the licensed layers to ECR allowing these images to be pulled from ECR via the VPC Endpoint without additional access to Azure being required.

**Important**
Usage of this flag shall not preclude your obligation to comply with the terms of the Windows container base image license; you must not post Windows content for public or third-party redistribution. Usage within your own environment is allowed.

To enable this flag in your development or build machine you will want to need to modify the `daemon.json` configuration file which can be found under `C:\ProgramData\docker\config\daemon.json`. The following example demonstrates this configuration:

```
{
	"allow-nondistributable-artifacts" : [
		"1234567890.dkr.ecr.region.amazonaws.com"
	]
}
```

After modifying `daemon.json` restart the Docker daemon and then attempt to push your image. The base layer should be pushed to ECR as well.

**Note**
The base layers for images of the Windows Operating System type are quite large, approximately 9GiB. Please note that this will result in a longer time to push and additional storage costs in ECR for these images. We therefore recommend only using this option when it is strictly required to reduce build times and ongoing storage costs.

