# Private registry authentication<a name="registry_auth"></a>

You can use the AWS Management Console, the AWS CLI, or the AWS SDKs to create and manage private repositories\. You can also use those methods to perform some actions on images, such as listing or deleting them\. These clients use standard AWS authentication methods\. Even though you can use the Amazon ECR API to push and pull images, you're more likely to use the Docker CLI or a language\-specific Docker library\.

The Docker CLI doesn't support native IAM authentication methods\. Additional steps must be taken so that Amazon ECR can authenticate and authorize Docker push and pull requests\.

The registry authentication methods that are detailed in the following sections are available\.

## Using the Amazon ECR credential helper<a name="registry-auth-credential-helper"></a>

Amazon ECR provides a Docker credential helper which makes it easier to store and use Docker credentials when pushing and pulling images to Amazon ECR\. For installation and configuration steps, see [Amazon ECR Docker Credential Helper](https://github.com/awslabs/amazon-ecr-credential-helper)\.

**Note**  
The Amazon ECR Docker credential helper doesn't support multi\-factor authentication \(MFA\) currently\.

## Using an authorization token<a name="registry-auth-token"></a>

An authorization token's permission scope matches that of the IAM principal used to retrieve the authentication token\. An authentication token is used to access any Amazon ECR registry that your IAM principal has access to and is valid for 12 hours\. To obtain an authorization token, you must use the [GetAuthorizationToken](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API operation to retrieve a base64\-encoded authorization token containing the username `AWS` and an encoded password\. The AWS CLI `get-login-password` command simplifies this by retrieving and decoding the authorization token which you can then pipe into a docker login command to authenticate\.

### To authenticate Docker to an Amazon ECR private registry with the CLI<a name="get-login-password"></a>

To authenticate Docker to an Amazon ECR registry with get\-login\-password, run the aws ecr get\-login\-password command\. When passing the authentication token to the docker login command, use the value `AWS` for the username and specify the Amazon ECR registry URI you want to authenticate to\. If authenticating to multiple registries, you must repeat the command for each registry\.
**Important**  
If you receive an error, install or upgrade to the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) in the *AWS Command Line Interface User Guide*\.
+ [get\-login\-password](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login-password.html) \(AWS CLI\)

  ```
  aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
  ```
+ [Get\-ECRLoginCommand](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRLoginCommand.html) \(AWS Tools for Windows PowerShell\)

  ```
  (Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
  ```

## Using HTTP API authentication<a name="registry_auth_http"></a>

Amazon ECR supports the [Docker Registry HTTP API](https://docs.docker.com/registry/spec/api/)\. However, because Amazon ECR is a private registry, you must provide an authorization token with every HTTP request\. You can add an HTTP authorization header using the `-H` option for curl and pass the authorization token provided by the get\-authorization\-token AWS CLI command\.

**To authenticate with the Amazon ECR HTTP API**

1. Retrieve an authorization token with the AWS CLI and set it to an environment variable\.

   ```
   TOKEN=$(aws ecr get-authorization-token --output text --query 'authorizationData[].authorizationToken')
   ```

1. To authenticate to the API, pass the `$TOKEN` variable to the `-H` option of curl\. For example, the following command lists the image tags in an Amazon ECR repository\. For more information, see the [Docker Registry HTTP API](https://docs.docker.com/registry/spec/api/) reference documentation\.

   ```
   curl -i -H "Authorization: Basic $TOKEN" https://aws_account_id.dkr.ecr.region.amazonaws.com/v2/amazonlinux/tags/list
   ```

   The output is as follows:

   ```
   HTTP/1.1 200 OK
   Content-Type: text/plain; charset=utf-8
   Date: Thu, 04 Jan 2018 16:06:59 GMT
   Docker-Distribution-Api-Version: registry/2.0
   Content-Length: 50
   Connection: keep-alive
   
   {"name":"amazonlinux","tags":["2017.09","latest"]}
   ```