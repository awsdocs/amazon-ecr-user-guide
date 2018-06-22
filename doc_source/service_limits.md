# Amazon ECR Service Limits<a name="service_limits"></a>

The following table provides the default limits for Amazon ECR for an AWS account which can be changed\. For more information, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.


| Resource | Default Limit | 
| --- | --- | 
| Maximum number of repositories per account | 1,000 | 
| Maximum number of images per repository | 1,000 | 
| Maximum [http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API transactions per second, per account, per region | 20 sustained, with the ability to burst up to 200 \* | 

\* In each region, each account receives a bucket that can store up to 200 [http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) credits\. These credits are replenished at a rate of 20 per second\. If your bucket has 200 credits, you could achieve 200 [http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API transactions per second for one second, and then sustain 20 transactions per second indefinitely\.

The following table provides other limitations for Amazon ECR and Docker images that cannot be changed\.

**Note**  
The layer part information mentioned in the table below is only applicable to customers who are calling the Amazon ECR APIs directly to initiate multipart uploads for image push operations \(we expect this to be very rare\)\.  
We recommend that you use the docker CLI to pull, tag, and push images\.


| Resource | Default Limit | 
| --- | --- | 
| Maximum number of layers per image | 127 \(this is the current Docker limit\) | 
| Maximum number of tags per image | 100 | 
| Maximum layer size \*\* | 10,000 MiB | 
| Maximum layer part size | 10 MiB | 
| Minimum layer part size | 5 MiB \(except the final layer part in an upload\) | 
| Maximum number of layer parts | 1,000 | 

\*\* The maximum layer size listed here is calculated by multiplying the maximum layer part size \(10 MiB\) by the maximum number of layer parts \(1,000\)\.