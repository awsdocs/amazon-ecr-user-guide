# Amazon ECR Service Limits<a name="service_limits"></a>

The following table provides the default limits for Amazon Elastic Container Registry \(Amazon ECR\) for an AWS account that can be changed\. For more information, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.


| Resource | Default Limit | 
| --- | --- | 
| Maximum number of repositories per region | 1,000 | 
| Maximum number of images per repository | 1,000 | 
| Number of [https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API transactions per second, per region, per account | 20 sustained, with the ability to burst up to 200 \* | 
| Number of docker pull transactions to a repository per second, per region, per account | 200 sustained, with the ability to burst up to 400 \*  | 
| Number of docker pull layer transactions to a repository per second, per region, per account | 200 sustained, with the ability to burst up to 400 \* | 
| Number of docker push transactions to a repository per second, per region, per account | 10 sustained, with the ability to burst up to 40 \* | 

\* In each region, each account receives a bucket that can store up to a specific number of credits, depending on the transaction\. These credits are replenished at the specified sustain rate per second\. For example, for `GetAuthorizationToken` API transactions, your bucket can store up to 200 credits\. You could achieve 200 `GetAuthorizationToken` API transactions per second for one second, and then sustain 20 transactions per second indefinitely\.

The following table provides other limitations for Amazon ECR and Docker images that cannot be changed\.

**Note**  
The layer part information mentioned in the table below is only applicable to customers who are calling the Amazon ECR API actions directly to initiate multipart uploads for image push operations\. We expect this to be rare\.  
We recommend that you use the docker CLI to pull, tag, and push images\.


| Resource | Default Limit | 
| --- | --- | 
| Maximum number of layers per image | 127 \(this is the current Docker limit\) | 
| Maximum number of tags per image | 100 | 
| Maximum layer size \*\* | 10,000 MiB | 
| Maximum layer part size | 10 MiB | 
| Minimum layer part size | 5 MiB \(except the final layer part in an upload\) | 
| Maximum number of layer parts | 1,000 | 
| Maximum number of rules in a lifecycle policy | 50 | 
| Maximum length of a lifecycle policy | 30720 | 

\*\* The maximum layer size listed here is calculated by multiplying the maximum layer part size \(10 MiB\) by the maximum number of layer parts \(1,000\)\.