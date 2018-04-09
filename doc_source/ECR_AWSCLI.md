# Using the AWS CLI with Amazon ECR<a name="ECR_AWSCLI"></a>

The following steps will help you install the AWS CLI and then log in to Amazon ECR, create an image repository, push an image to that repository, and perform other common scenarios in Amazon ECR with the AWS CLI\.

The AWS Command Line Interface \(CLI\) is a unified tool to manage your AWS services\. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts\. For more information on the AWS CLI, see [http://aws\.amazon\.com/cli/](http://aws.amazon.com/cli/)\.

For more information on the other tools available for managing your AWS resources, including the different AWS SDKs, IDE toolkits, and the Windows PowerShell command line tools, see [http://aws\.amazon\.com/tools/](http://aws.amazon.com/tools/)\.

**Topics**
+ [Step 1: Authenticate Docker to your Default Registry](#AWSCLI_get-login)
+ [Step 2: Get a Docker Image](#AWSCLI_get_docker_image)
+ [Step 3: Create a Repository](#AWSCLI_create_repository)
+ [Step 4: Push an Image to Amazon ECR](#AWSCLI_push_image)
+ [Step 5: Pull an Image from Amazon ECR](#AWSCLI_pull_image)
+ [Step 6: Delete an Image](#AWSCLI_delete_image)
+ [Step 7: Delete a Repository](#AWSCLI_delete_repository)

## Step 1: Authenticate Docker to your Default Registry<a name="AWSCLI_get-login"></a>

After you have installed and configured the AWS CLI, you can authenticate the Docker CLI to your default registry so that the docker command can push and pull images with Amazon ECR\. The AWS CLI provides a get\-login command to simplify the authentication process\.

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
When you execute this docker login command, the command string can be visible by other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way and use them to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

## Step 2: Get a Docker Image<a name="AWSCLI_get_docker_image"></a>

Before you can push an image to Amazon ECR, you need to have one to push\. If you do not already have an image to use, you can create one by following the steps in [Docker Basics for Amazon ECR](docker-basics.md), or you can simply pull an image from Docker Hub that you would like to have in your Amazon ECR registry\. To pull the `ubuntu:trusty` image from Docker hub to your local system, run the following command:

```
$ docker pull ubuntu:trusty
trusty: Pulling from library/ubuntu
0a85502c06c9: Pull complete
0998bf8fb9e9: Pull complete
a6785352b25c: Pull complete
e9ae3c220b23: Pull complete
Digest: sha256:3cb273da02362a6e667b54f6cf907edd5255c706f9de279c97cfccc7c6988124
Status: Downloaded newer image for ubuntu:trusty
```

## Step 3: Create a Repository<a name="AWSCLI_create_repository"></a>

Now that you have an image to push to Amazon ECR, you need to create a repository to hold it\. In this example, you create a repository called `ubuntu` to which you later push the `ubuntu:trusty` image\. To create a repository, run the following command:

```
$ aws ecr create-repository --repository-name ubuntu
{
    "repository": {
        "registryId": "111122223333",
        "repositoryName": "ubuntu",
        "repositoryArn": "arn:aws:ecr:us-east-1:111122223333:repository/ubuntu"
    }
}
```

## Step 4: Push an Image to Amazon ECR<a name="AWSCLI_push_image"></a>

Now you can push your image to the Amazon ECR repository you created in the previous section\. You use the docker CLI to push images, but there are a few prerequisites that must be satisfied for this to work properly:
+ The minimum version of docker is installed: 1\.7
+ The Amazon ECR authorization token has been configured with docker login\.
+ The Amazon ECR repository exists and the user has access to push to the repository\.

After those prerequisites are met, you can push your image to your newly created repository in the default registry for your account\.

**To tag and push an image to Amazon ECR**

1. List the images you have stored locally to identify the image to tag and push\.

   ```
   $ docker images
   REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
   ubuntu              trusty              e9ae3c220b23        3 weeks ago         187.9 MB
   ```

1. Tag the image to push to your repository\.

   ```
   $ docker tag ubuntu:trusty aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
   ```

1. Push the image\.

   ```
   $ docker push aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
   The push refers to a repository [aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu] (len: 1)
   e9ae3c220b23: Pushed
   a6785352b25c: Pushed
   0998bf8fb9e9: Pushed
   0a85502c06c9: Pushed
   trusty: digest: sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b size: 6774
   ```

## Step 5: Pull an Image from Amazon ECR<a name="AWSCLI_pull_image"></a>

After your image has been pushed to your Amazon ECR repository, you can pull it from other locations\. We will use the docker CLI to pull images, but there are a few prerequisites that must be satisfied for this to work properly:
+ The minimum version of docker is installed: 1\.7
+ The Amazon ECR authorization token has been configured with docker login\.
+ The Amazon ECR repository exists and the user has access to pull from the repository\.

After those prerequisites are met, you can pull your image\. To pull your example image from Amazon ECR, run the following command:

```
$ docker pull aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
trusty: Pulling from ubuntu
0a85502c06c9: Pull complete
0998bf8fb9e9: Pull complete
a6785352b25c: Pull complete
e9ae3c220b23: Pull complete
Digest: sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b
Status: Downloaded newer image for aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
```

## Step 6: Delete an Image<a name="AWSCLI_delete_image"></a>

If you decide that you no longer need or want an image in one of your repositories, you can delete it with the batch\-delete\-image command\. To delete an image, you must specify the repository that it is in and either a `imageTag` or `imageDigest` value for the image\. The example below deletes an image in the `ubuntu` repository with the image tag `trusty`\.

```
$ aws ecr batch-delete-image --repository-name ubuntu --image-ids imageTag=trusty
{
    "failures": [],
    "imageIds": [
        {
            "imageTag": "trusty",
            "imageDigest": "sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b"
        }
    ]
}
```

## Step 7: Delete a Repository<a name="AWSCLI_delete_repository"></a>

If you decide that you no longer need or want an entire repository of images, you can delete the repository\. By default, you cannot delete a repository that contains images; however, the `--force` flag allows this\. To delete a repository that contains images \(and all the images within it\), run the following command:

```
 $ aws ecr delete-repository --repository-name ubuntu --force
{
    "repository": {
        "registryId": "aws_account_id",
        "repositoryName": "ubuntu",
        "repositoryArn": "arn:aws:ecr:us-east-1:aws_account_id:repository/ubuntu",
        "createdAt": 1457671643.0,
        "repositoryUri": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu"
    }
}
```