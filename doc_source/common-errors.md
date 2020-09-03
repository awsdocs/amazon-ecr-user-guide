# Troubleshooting Amazon ECR Error Messages<a name="common-errors"></a>

In some cases, an API call that you have triggered through the Amazon ECS console or the AWS CLI exits with an error message\. Some common error messages and potential solutions are explained below\. 

## Error: "Error Response from Daemon: Invalid Registry Endpoint" When Running aws ecr get\-login<a name="error-invalid-registry-endpoint"></a>

 You may see the following error when running the aws ecr get\-login command to obtain the login credentials for your Amazon ECR repository: 

```
Error response from daemon: invalid registry endpoint 
    https://xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/v0/: unable to ping registry endpoint 
    https://xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/v0/
v2 ping attempt failed with error: Get https://xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/v2/: 
    dial tcp: lookup xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com on 172.20.10.1:53: 
    read udp 172.20.10.1:53: i/o timeout
```

This error can occur on MacOS X and Windows systems that are running Docker Toolbox, Docker for Windows, or Docker for Mac\. It is often caused when other applications alter the routes through the local gateway \(192\.168\.0\.1\) through which the virtual machine must call to access the Amazon ECR service\. If this error occurs when using Docker Toolbox, then it can often be resolved by restarting the Docker Machine environment, or rebooting the local client operating system\. If this does not resolve the issue, use the docker\-machine ssh command to log in to your container instance\. Perform a DNS lookup on an external host to verify that you see the same results as you see on your local host\. If the results differ, consult the documentation for Docker Toolbox to ensure that your Docker Machine environment is configured properly\. 

## HTTP 429: Too Many Requests or ThrottleException<a name="error-429-too-many-requests"></a>

You may receive a `429: Too Many Requests` error or a `ThrottleException` error from one or more Amazon ECR commands or API calls\. If you are using Docker tools with Amazon ECR, then for Docker versions 1\.12\.0 and greater, you may see the error message `TOOMANYREQUESTS: Rate exceeded`\. For versions of Docker below 1\.12\.0, you may see the error `Unknown: Rate exceeded`\.

This indicates that you are calling a single endpoint in Amazon ECR repeatedly over a short interval, and that your requests are getting throttled\. Throttling occurs when calls to a single endpoint from a single user exceed a certain threshold over a period of time\.

Various API operations in Amazon ECR have different throttles\.

For example, the throttle for the [https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) action is 20 transaction per second \(TPS\), with up to a 200 TPS burst allowed\. In each region, each account receives a bucket that can store up to 200 `GetAuthorizationToken` credits\. These credits are replenished at a rate of 20 per second\. If your bucket has 200 credits, you could achieve 200 `GetAuthorizationToken` API transactions per second for one second, and then sustain 20 transactions per second indefinitely\.

To handle throttling errors, implement a retry function with incremental backoff into your code\. For more information, see [Error Retries and Exponential Backoff in AWS](https://docs.aws.amazon.com/general/latest/gr/api-retries.html) in the [Amazon Web Services General Reference](https://docs.aws.amazon.com/general/latest/gr/)\.

## HTTP 403: "User \[arn\] is not authorized to perform \[operation\]"<a name="error-unauthorized"></a>

You may receive the following error when attempting to perform an action with Amazon ECR:

```
$ aws ecr get-login 
A client error (AccessDeniedException) occurred when calling the GetAuthorizationToken operation: 
    User: arn:aws:iam::account-number:user/username is not authorized to perform: 
    ecr:GetAuthorizationToken on resource: *
```

This indicates that your user does not have permissions granted to use Amazon ECR, or that those permissions are not set up correctly\. In particular, if you are performing actions against an Amazon ECR repository, verify that the user has been granted permissions to access that repository\. For more information about creating and verifying permissions for Amazon ECR, see [Identity and Access Management for Amazon Elastic Container Registry](security-iam.md)\.

## HTTP 404: "Repository Does Not Exist" Error<a name="repo-does-not-exist-error"></a>

If you specify a Docker Hub repository that does not currently exist, Docker Hub creates it automatically\. With Amazon ECR, new repositories must be explicitly created before they can be used\. This prevents new repositories from being created accidentally \(for example, due to typos\), and it also ensures that an appropriate security access policy is explicitly assigned to any new repositories\. For more information about creating repositories, see [Amazon ECR repositories](Repositories.md)\.