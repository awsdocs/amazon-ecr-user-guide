# Troubleshooting Errors with Docker Commands When Using Amazon ECR<a name="common-errors-docker"></a>

**Topics**
+ [Error: "Filesystem Verification Failed" or "404: Image Not Found" When Pulling an Image From an Amazon ECR Repository](#error-filesystem-verification-failed)
+ [Error: "Filesystem Layer Verification Failed" When Pulling Images from Amazon ECR](#error-filesystem-layer-verification)
+ [HTTP 403 Errors or "no basic auth credentials" Error When Pushing to Repository](#error-403)

In some cases, running a Docker command against Amazon ECR may result in an error message\. Some common error messages and potential solutions are explained below\. 

## Error: "Filesystem Verification Failed" or "404: Image Not Found" When Pulling an Image From an Amazon ECR Repository<a name="error-filesystem-verification-failed"></a>

You may receive the error `Filesystem verification failed` when using the docker pull command to pull an image from an Amazon ECR repository with Docker 1\.9 or above, or the error `404: Image not found` when using Docker versions prior to 1\.9\. 

Some possible reasons and their explanations are given below\.

The local disk is full  
If the local disk on which you're running the docker pull command is full, then the SHA\-1 hash that is calculated on the locally downloaded file may be different than the SHA\-1 hash calculated by Amazon ECR\. Check that your local disk has enough remaining free space to store the Docker image you are pulling\. You can also delete old images to make room for new ones\. Use the docker images command to see a list of all locally downloaded Docker images, along with their sizes\. 

Client cannot connect to the remote repository due to network error  
Calls to an Amazon ECR repository require a functioning connection to the Internet\. Verify your network settings, and verify that other tools and applications can access resources on the Internet\. If you are running the docker pull command on an Amazon EC2 instance in a private subnet, verify that your private subnet has a route to the Internet through a network address translation \(NAT\) server or through a managed NAT gateway\.  
Currently, calls to an Amazon ECR repository also require network access through your corporate firewall to Amazon Simple Storage Service \(Amazon S3\)\. If your organization uses firewall software or a NAT device that white lists allowed service endpoints, ensure that the Amazon S3 service endpoints for your current region are included in the whitelist\.   
If you are using Docker behind an HTTP proxy, you can configure Docker with the appropriate proxy settings\. For more information, see [HTTP proxy](https://docs.docker.com/engine/admin/systemd/#/http-proxy) in the Docker documentation\. 

## Error: "Filesystem Layer Verification Failed" When Pulling Images from Amazon ECR<a name="error-filesystem-layer-verification"></a>

You may receive the error `image image-name not found` when pulling images using the docker pull command\. If you inspect the Docker logs, you may see an error like the following:

```
filesystem layer verification failed for digest sha256:2b96f...
```

This error indicates that one or more of the layers for your image has failed to download\. Some possible reasons and their explanations are given below\.

You are using an older version of Docker  
This error can occur in a small percentage of cases when using a Docker version less than 1\.10\. Upgrade your Docker client to 1\.10 or greater\.

Your client has encountered a network or disk error  
 A full disk or a network issue may prevent one or more layers from downloading, as discussed earlier about the `Filesystem verification failed` message,\. Follow the recommendations above to ensure that your filesystem is not full, and that you have enabled access to Amazon S3 from within your network\.

## HTTP 403 Errors or "no basic auth credentials" Error When Pushing to Repository<a name="error-403"></a>

There are times when you may receive an HTTP 403 \(Forbidden\) error, or the error message `no basic auth credentials` from the docker push command, even if you have successfully authenticated to Docker using the aws ecr get\-login command\. The following are some known causes of this issue:

You have authenticated to a different region   
Authentication requests are tied to specific regions, and cannot be used across regions\. For example, if you obtain an authorization token from US West \(Oregon\), you cannot use it to authenticate against your repositories in US East \(N\. Virginia\)\. To resolve the issue, ensure that you are using the same region for both authentication and docker push command calls\.

You have authenticated to push to a repository on the wrong AWS account   
If you are using an IAM user from one AWS account, but you are attempting to push to a repository hosted in another account, you will need to explicitly specify the `--registry-ids` parameter when you call `aws ecr get-login`\. Otherwise, you will by default get a Docker login command which only authorizes you to push to repositories on the same account that hosts your IAM user, not the account that hosts your repository\. Always ensure that the repository URL in the response from `aws ecr get-login` matches the repository URL that you are pushing to, including the account ID portion of the URL\.

Your token has expired   
The default token expiration period for tokens obtained using the `GetAuthorizationToken` operation is 12 hours\. However, if you use a temporary security credential mechanism such as multi\-factor authentication \(MFA\) or AWS Security Token Service to authenticate and receive your token, the expiration period of the Amazon ECR authorization token is equal to the duration of these temporary security credentials\. For example, if you call the aws ecr get\-login command by assuming a role, the authorization token expires within 15 minutes to 1 hour, depending on the settings you use when calling the aws sts assume\-role command\. 

Bug in `wincred` credential manager  
Some versions of Docker for Windows use a credential manager called `wincred`, which does not properly handle the Docker login command produced by aws ecr get\-login \(for more information, see [https://github\.com/docker/docker/issues/22910](https://github.com/docker/docker/issues/22910)\)\. You can run the Docker login command that is output, but when you try to push or pull images, those commands will fail\. You can work around this bug by removing the `https://` scheme from the registry argument in the Docker login command that is output from aws ecr get\-login\. An example Docker login command without the HTTPS scheme is shown below\.  

```
docker login -u AWS -p <password> <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```