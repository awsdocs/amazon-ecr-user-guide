# Troubleshooting Amazon ECR error messages<a name="common-errors"></a>

In some cases, an API call that you have triggered through the Amazon ECS console or the AWS CLI exits with an error message\. Some common error messages and potential solutions are explained below\. 

## HTTP 429: Too Many Requests or ThrottleException<a name="error-429-too-many-requests"></a>

You may receive a `429: Too Many Requests` error or a `ThrottleException` error from one or more Amazon ECR commands or API calls\. If you are using Docker tools with Amazon ECR, then for Docker versions 1\.12\.0 and greater, you may see the error message `TOOMANYREQUESTS: Rate exceeded`\. For versions of Docker below 1\.12\.0, you may see the error `Unknown: Rate exceeded`\.

This indicates that you are calling a single endpoint in Amazon ECR repeatedly over a short interval, and that your requests are getting throttled\. Throttling occurs when calls to a single endpoint from a single user exceed a certain threshold over a period of time\.

Various API operations in Amazon ECR have different throttles\.

For example, the throttle for the [https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) action is 20 transaction per second \(TPS\), with up to a 200 TPS burst allowed\. In each region, each account receives a bucket that can store up to 200 `GetAuthorizationToken` credits\. These credits are replenished at a rate of 20 per second\. If your bucket has 200 credits, you could achieve 200 `GetAuthorizationToken` API transactions per second for one second, and then sustain 20 transactions per second indefinitely\.

To handle throttling errors, implement a retry function with incremental backoff into your code\. For more information, see [Error Retries and Exponential Backoff in AWS](https://docs.aws.amazon.com/general/latest/gr/api-retries.html) in the [Amazon Web Services General Reference](https://docs.aws.amazon.com/general/latest/gr/)\.

## HTTP 403: "User \[arn\] is not authorized to perform \[operation\]"<a name="error-unauthorized"></a>

You may receive the following error when attempting to perform an action with Amazon ECR:

```
$ aws ecr get-login-password 
A client error (AccessDeniedException) occurred when calling the GetAuthorizationToken operation: 
    User: arn:aws:iam::account-number:user/username is not authorized to perform: 
    ecr:GetAuthorizationToken on resource: *
```

This indicates that your user does not have permissions granted to use Amazon ECR, or that those permissions are not set up correctly\. In particular, if you are performing actions against an Amazon ECR repository, verify that the user has been granted permissions to access that repository\. For more information about creating and verifying permissions for Amazon ECR, see [Identity and Access Management for Amazon Elastic Container Registry](security-iam.md)\.

## HTTP 404: "Repository Does Not Exist" error<a name="repo-does-not-exist-error"></a>

If you specify a Docker Hub repository that does not currently exist, Docker Hub creates it automatically\. With Amazon ECR, new repositories must be explicitly created before they can be used\. This prevents new repositories from being created accidentally \(for example, due to typos\), and it also ensures that an appropriate security access policy is explicitly assigned to any new repositories\. For more information about creating repositories, see [Amazon ECR private repositories](Repositories.md)\.