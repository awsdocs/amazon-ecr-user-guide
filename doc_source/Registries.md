# Amazon ECR Registries<a name="Registries"></a>

You can use Amazon ECR registries to host your images in a highly available and scalable architecture, allowing you to deploy containers reliably for your applications\. You can use your registry to manage image repositories and Docker images\. Each AWS account is provided with a single \(default\) Amazon ECR registry\.

## Registry Concepts<a name="registry_concepts"></a>
+ The URL for your default registry is `https://``aws_account_id.dkr.ecr.region.amazonaws.com`\.
+ By default, you have read and write access to the repositories and images you create in your default registry\.
+ You must authenticate your Docker client to a registry so that you can use the docker push and docker pull commands to push and pull images to and from the repositories in that registry\. For more information, see [Registry Authentication](#registry_auth)\.
+ Repositories can be controlled with both IAM user access policies and repository policies\.

## Registry Authentication<a name="registry_auth"></a>

You can use the AWS Management Console, the AWS CLI, or the AWS SDKs to create and manage repositories, and to perform some actions on images, such as listing or deleting them\. These clients use standard AWS authentication methods\. Although technically you can use the Amazon ECR API to push and pull images, you are much more likely to use Docker CLI \(or a language\-specific Docker library\) for these purposes\.

Because the Docker CLI does not support the standard AWS authentication methods, you must authenticate your Docker client another way so that Amazon ECR knows who is requesting to push or pull an image\. If you are using the Docker CLI, then use the docker login command to authenticate to an Amazon ECR registry with an authorization token that is provided by Amazon ECR and is valid for 12 hours\. The [GetAuthorizationToken](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetAuthorizationToken.html) API operation provides a base64\-encoded authorization token that contains a user name \(`AWS`\) and a password that you can decode and use in a docker login command\. However, a much simpler get\-login command \(which retrieves the token, decodes it, and converts it to a docker login command for you\) is available in the AWS CLI\.

**To authenticate Docker to an Amazon ECR registry with get\-login**
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. You can check your AWS CLI version with the aws \-\-version command\.

1. Run the aws ecr get\-login command\. The example below is for the default registry associated with the account making the request\. To access other account registries, use the `--registry-ids aws_account_id` option\. For more information, see [get\-login](http://docs.aws.amazon.com/cli/latest/reference/ecr/get-login.html) in the *AWS CLI Command Reference*\.

   ```
   aws ecr get-login --no-include-email
   ```

   Output:

   ```
   docker login -u AWS -p password https://aws_account_id.dkr.ecr.us-east-1.amazonaws.com
   ```
**Important**  
If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

   The resulting output is a docker login command that you use to authenticate your Docker client to your Amazon ECR registry\.

1. Copy and paste the docker login command into a terminal to authenticate your Docker CLI to the registry\. This command provides an authorization token that is valid for the specified registry for 12 hours\. 
**Note**  
If you are using Windows PowerShell, copying and pasting long strings like this does not work\. Use the following command instead\.  

   ```
   Invoke-Expression -Command (aws ecr get-login --no-include-email)
   ```
**Important**  
When you execute this docker login command, the command string can be visible to other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way and use them to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

## HTTP API Authentication<a name="registry_auth_http"></a>

Amazon ECR supports the [Docker Registry HTTP API](https://docs.docker.com/registry/spec/api/)\. However, because Amazon ECR is a private registry, you must provide an authorization token with every HTTP request\. You can add an HTTP authorization header using the `-H` option for curl and pass the authorization token provided by the get\-authorization\-token AWS CLI command\.

**To authenticate with the Amazon ECR HTTP API**

1. Retrieve an authorization token with the AWS CLI and set it to an environment variable\.

   ```
   TOKEN=$(aws ecr get-authorization-token --output text --query 'authorizationData[].authorizationToken')
   ```

1. Pass the `$TOKEN` variable to the `-H` option of curl to authenticate to the API\. For example, the following command lists the image tags in an Amazon ECR repository\. For more information, see the [Docker Registry HTTP API](https://docs.docker.com/registry/spec/api/) reference documentation\.

   ```
   curl -i -H "Authorization: Basic $TOKEN" https://012345678910.dkr.ecr.us-east-1.amazonaws.com/v2/amazonlinux/tags/list
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