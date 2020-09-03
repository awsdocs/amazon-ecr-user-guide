# Amazon ECR registries<a name="Registries"></a>

Amazon ECR registries host your container images in a highly available and scalable architecture, allowing you to deploy containers reliably for your applications\. You can use your registry to manage image repositories consisting of Docker and Open Container Initiative \(OCI\) images\. Each AWS account is provided with a single \(default\) Amazon ECR registry\.

## Registry concepts<a name="registry_concepts"></a>
+ The URL for your default registry is `https://``aws_account_id.dkr.ecr.region.amazonaws.com`\.
+ By default, your account has read and write access to the repositories in your default registry\. However, IAM users require permissions to make calls to the Amazon ECR APIs and to push or pull images from your repositories\. Amazon ECR provides several managed policies to control user access at varying levels\. For more information, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.
+ You must authenticate your Docker client to a registry so that you can use the docker push and docker pull commands to push and pull images to and from the repositories in that registry\. For more information, see [Registry authentication](#registry_auth)\.
+ Repositories can be controlled with both IAM user access policies and repository policies\. For more information about repository policies, see [Repository policies](repository-policies.md)\.

## Registry authentication<a name="registry_auth"></a>

You can use the AWS Management Console, the AWS CLI, or the AWS SDKs to create and manage repositories\. You can also use those methods to perform some actions on images, such as listing or deleting them\. These clients use standard AWS authentication methods\. Although technically you can use the Amazon ECR API to push and pull images, you are much more likely to use the Docker CLI or a language\-specific Docker library\.

The Docker CLI does not support native IAM authentication methods\. Additional steps must be taken so that Amazon ECR can authenticate and authorize Docker push and pull requests\.

The following registry authentication methods are available\.

### Using the Amazon ECR credential helper<a name="registry-auth-credential-helper"></a>

Amazon ECR provides a Docker credential helper which makes it easier to store and use Docker credentials when pushing and pulling images to Amazon ECR\. For installation and configuration steps, see [Amazon ECR Docker Credential Helper](https://github.com/awslabs/amazon-ecr-credential-helper)\.

### Using an authorization token<a name="registry-auth-token"></a>

An authorization token's permission scope matches that of the IAM principal used to retrieve the authentication token\. An authentication token is used to access any Amazon ECR registry that your IAM principal has access to and is valid for 12 hours\. To obtain an authorization token, you must use the [GetAuthorizationToken](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API operation to retrieve a base64\-encoded authorization token containing the username `AWS` and an encoded password\. The AWS CLI `get-login-password` command simplifies this by retrieving and decoding the authorization token which you can then pipe into a docker login command to authenticate\.

#### To authenticate Docker to an Amazon ECR registry with get\-login\-password<a name="get-login-password"></a>

To authenticate Docker to an Amazon ECR registry with get\-login\-password, run the aws ecr get\-login\-password command\. When passing the authentication token to the docker login command, use the value `AWS` for the username and specify the Amazon ECR registry URI you want to authenticate to\. If authenticating to multiple registries, you must repeat the command for each registry\.
**Important**  
If you receive an error, install or upgrade to the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.
+ [get\-login\-password](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login-password.html) \(AWS CLI\)

  ```
  aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
  ```
+ [Get\-ECRLoginCommand](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECRLoginCommand.html) \(AWS Tools for Windows PowerShell\)

  ```
  (Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
  ```

#### To authenticate Docker to an Amazon ECR registry with get\-login<a name="get-login"></a>

When using AWS CLI versions prior to 1\.17\.10, the get\-login command is available to authenticate to your Amazon ECR registry\. You can check your AWS CLI version with the aws \-\-version command\.

1. Run the aws ecr get\-login command\. The example below is for the default registry associated with the account making the request\. To access other account registries, use the `--registry-ids aws_account_id` option\. For more information, see [get\-login](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login.html) in the *AWS CLI Command Reference*\.

   ```
   aws ecr get-login --region region --no-include-email
   ```

   The resulting output is a docker login command that you use to authenticate your Docker client to your Amazon ECR registry\.

   ```
   docker login -u AWS -p password https://aws_account_id.dkr.ecr.region.amazonaws.com
   ```

1. Copy and paste the docker login command into a terminal to authenticate your Docker CLI to the registry\. This command provides an authorization token that is valid for the specified registry for 12 hours\.
**Note**  
If you are using Windows PowerShell, copying and pasting long strings like this does not work\. Use the following command instead\.  

   ```
   Invoke-Expression -Command (Get-ECRLoginCommand -Region region).Command
   ```
**Important**  
When you execute this docker login command, the command string can be visible to other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way\. They could use the credentials to gain push and pull access to your repositories\. If you are not on a secure system, you should use the ecr get\-login\-password command as described above\.

### Using HTTP API authentication<a name="registry_auth_http"></a>

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

   Output:

   ```
   HTTP/1.1 200 OK
   Content-Type: text/plain; charset=utf-8
   Date: Thu, 04 Jan 2018 16:06:59 GMT
   Docker-Distribution-Api-Version: registry/2.0
   Content-Length: 50
   Connection: keep-alive
   
   {"name":"amazonlinux","tags":["2017.09","latest"]}
   ```