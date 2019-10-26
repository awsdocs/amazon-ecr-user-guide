# Minimum Amazon S3 Bucket Permissions for Amazon ECR<a name="ecr-minimum-s3-perms"></a>

The Amazon S3 gateway endpoint uses an IAM policy document to limit access to the service\. To allow only the minimum Amazon S3 bucket permissions for Amazon ECR, restrict access to the Amazon S3 bucket that Amazon ECR uses when you create the IAM policy document for the endpoint\. 

The following table describes the Amazon S3 bucket policy permissions needed by Amazon ECR\.


| Permission | Description | 
| --- | --- | 
| arn:aws:s3:::prod\-region\-starport\-layer\-bucket/\* | Provides access to the Amazon S3 bucket containing the layers for each Docker image\. Represents the Region identifier for an AWS Region supported by Amazon ECR, such as us\-east\-2 for the US East \(Ohio\) Region\. | 

## Example<a name="ecr-minimum-s3-perms-example"></a>

The following example illustrates how to provide access to the Amazon S3 buckets required for Amazon ECR operations\.

```
{
  "Statement": [
    {
      "Sid": "Access-to-specific-bucket-only",
      "Principal": "*",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": ["arn:aws:s3:::prod-region-starport-layer-bucket/*"]
    }
  ]
}
```