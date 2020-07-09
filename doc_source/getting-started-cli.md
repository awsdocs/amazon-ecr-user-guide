# Getting started with Amazon ECR using the AWS CLI<a name="getting-started-cli"></a>

The following steps walk you through the steps needed to push a container image to Amazon ECR for the first time using the Docker CLI and the AWS CLI\.

For more information on the other tools available for managing your AWS resources, including the different AWS SDKs, IDE toolkits, and the Windows PowerShell command line tools, see [http://aws\.amazon\.com/tools/](http://aws.amazon.com/tools/)\.

## Prerequisites<a name="getting-started-cli-prereqs"></a>

Before you begin, be sure that you have completed the steps in [Setting up with Amazon ECR](get-set-up-for-amazon-ecr.md)\.

If you do not already have the latest AWS CLI and Docker installed and ready to use, use the following steps to install both of these tools\.

### Install the AWS CLI<a name="cli-install"></a>

You can use the AWS command line tools to issue commands at your system's command line to perform Amazon ECR and other AWS tasks\. This can be faster and more convenient than using the console\. The command line tools are also useful for building scripts that perform AWS tasks\.

To use the AWS CLI with Amazon ECR, install the latest AWS CLI version \(Amazon ECR functionality is available in the AWS CLI starting with version 1\.9\.15\)\. You can check your AWS CLI version with the aws \-\-version command\. For information about installing the AWS CLI or upgrading it to the latest version, see [Installing the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) in the *AWS Command Line Interface User Guide*\.

### Install Docker<a name="cli-install-docker"></a>

Docker is available on many different operating systems, including most modern Linux distributions, like Ubuntu, and even Mac OSX and Windows\. For more information about how to install Docker on your particular operating system, go to the [Docker installation guide](https://docs.docker.com/engine/installation/#installation)\.

You don't need a local development system to use Docker\. If you are using Amazon EC2 already, you can launch an Amazon Linux 2 instance and install Docker to get started\.

If you already have Docker installed, skip to [Step 1: Create a Docker image](#cli-create-image)\.

**To install Docker on an Amazon EC2 instance**

1. Launch an instance with the Amazon Linux 2 AMI\. For more information, see [Launching an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Connect to your instance\. For more information, see [Connect to Your Linux Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Update the installed packages and package cache on your instance\.

   ```
   sudo yum update -y
   ```

1. Install the most recent Docker Community Edition package\.

   ```
   sudo amazon-linux-extras install docker
   ```

1. Start the Docker service\.

   ```
   sudo service docker start
   ```

1. Add the `ec2-user` to the `docker` group so you can execute Docker commands without using `sudo`\.

   ```
   sudo usermod -a -G docker ec2-user
   ```

1. Log out and log back in again to pick up the new `docker` group permissions\. You can accomplish this by closing your current SSH terminal window and reconnecting to your instance in a new one\. Your new SSH session will have the appropriate `docker` group permissions\.

1. Verify that the `ec2-user` can run Docker commands without `sudo`\.

   ```
   docker info
   ```
**Note**  
In some cases, you may need to reboot your instance to provide permissions for the `ec2-user` to access the Docker daemon\. Try rebooting your instance if you see the following error:  

   ```
   Cannot connect to the Docker daemon. Is the docker daemon running on this host?
   ```

## Step 1: Create a Docker image<a name="cli-create-image"></a>

In this section, you create a Docker image of a simple web application, and test it on your local system or EC2 instance, and then push the image to a container registry \(such as Amazon ECR or Docker Hub\) so you can use it in an ECS task definition\.

**To create a Docker image of a simple web application**

1. Create a file called `Dockerfile`\. A Dockerfile is a manifest that describes the base image to use for your Docker image and what you want installed and running on it\. For more information about Dockerfiles, go to the [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)\.

   ```
   touch Dockerfile
   ```

1. Edit the `Dockerfile` you just created and add the following content\.

   ```
   FROM ubuntu:18.04
   
   # Install dependencies
   RUN apt-get update && \
    apt-get -y install apache2
   
   # Install apache and write hello world message
   RUN echo 'Hello World!' > /var/www/html/index.html
   
   # Configure apache
   RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
    echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
    echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \ 
    echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \ 
    chmod 755 /root/run_apache.sh
   
   EXPOSE 80
   
   CMD /root/run_apache.sh
   ```

   This Dockerfile uses the Ubuntu 18\.04 image\. The `RUN` instructions update the package caches, install some software packages for the web server, and then write the "Hello World\!" content to the web server's document root\. The `EXPOSE` instruction exposes port 80 on the container, and the `CMD` instruction starts the web server\.

1. <a name="sample-docker-build-step"></a>Build the Docker image from your Dockerfile\.
**Note**  
Some versions of Docker may require the full path to your Dockerfile in the following command, instead of the relative path shown below\.

   ```
   docker build -t hello-world .
   ```

1. Run docker images to verify that the image was created correctly\.

   ```
   docker images --filter reference=hello-world
   ```

   Output:

   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   hello-world         latest              e9ffedc8c286        4 minutes ago       241MB
   ```

1. Run the newly built image\. The `-p 80:80` option maps the exposed port 80 on the container to port 80 on the host system\. For more information about docker run, go to the [Docker run reference](https://docs.docker.com/engine/reference/run/)\.

   ```
   docker run -t -i -p 80:80 hello-world
   ```
**Note**  
Output from the Apache web server is displayed in the terminal window\. You can ignore the "`Could not reliably determine the server's fully qualified domain name`" message\.

1. Open a browser and point to the server that is running Docker and hosting your container\.
   + If you are using an EC2 instance, this is the **Public DNS** value for the server, which is the same address you use to connect to the instance with SSH\. Make sure that the security group for your instance allows inbound traffic on port 80\.
   + If you are running Docker locally, point your browser to [http://localhost/](http://localhost/)\.
   + If you are using docker\-machine on a Windows or Mac computer, find the IP address of the VirtualBox VM that is hosting Docker with the docker\-machine ip command, substituting *machine\-name* with the name of the docker machine you are using\.

     ```
     docker-machine ip machine-name
     ```

   You should see a web page with your "Hello World\!" statement\.

1. Stop the Docker container by typing **Ctrl \+ c**\.

## Step 2: Authenticate to your default registry<a name="cli-authenticate-registry"></a>

After you have installed and configured the AWS CLI, authenticate the Docker CLI to your default registry\. That way, the docker command can push and pull images with Amazon ECR\. The AWS CLI provides a get\-login\-password command to simplify the authentication process\.

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

## Step 3: Create a repository<a name="cli-create-repository"></a>

Now that you have an image to push to Amazon ECR, you must create a repository to hold it\. In this example, you create a repository called `hello-world` to which you later push the `hello-world:latest` image\. To create a repository, run the following command:

```
aws ecr create-repository \
    --repository-name hello-world \
    --image-scanning-configuration scanOnPush=true \
    --region us-east-1
```

## Step 4: Push an image to Amazon ECR<a name="cli-push-image"></a>

Now you can push your image to the Amazon ECR repository you created in the previous section\. You use the docker CLI to push images, but there are a few prerequisites that must be satisfied for this to work properly:
+ The minimum version of docker is installed: 1\.7
+ The Amazon ECR authorization token has been configured with docker login\.
+ The Amazon ECR repository exists and the user has access to push to the repository\.

After those prerequisites are met, you can push your image to your newly created repository in the default registry for your account\.

**To tag and push an image to Amazon ECR**

1. List the images you have stored locally to identify the image to tag and push\.

   ```
   docker images
   ```

   Output:

   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
   hello-world         latest              e9ffedc8c286        4 minutes ago       241MB
   ```

1. Tag the image to push to your repository\.

   ```
   docker tag hello-world:latest aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest
   ```

1. Push the image\.

   ```
   docker push aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest
   ```

   Output:

   ```
   The push refers to a repository [aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world] (len: 1)
   e9ae3c220b23: Pushed
   a6785352b25c: Pushed
   0998bf8fb9e9: Pushed
   0a85502c06c9: Pushed
   latest: digest: sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b size: 6774
   ```

## Step 5: Pull an image from Amazon ECR<a name="cli-pull-image"></a>

After your image has been pushed to your Amazon ECR repository, you can pull it from other locations\. Use the docker CLI to pull images, but there are a few prerequisites that must be satisfied for this to work properly:
+ The minimum version of docker is installed: 1\.7
+ The Amazon ECR authorization token has been configured with docker login\.
+ The Amazon ECR repository exists and the user has access to pull from the repository\.

After those prerequisites are met, you can pull your image\. To pull your example image from Amazon ECR, run the following command:

```
docker pull aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest
```

Output:

```
latest: Pulling from hello-world
0a85502c06c9: Pull complete
0998bf8fb9e9: Pull complete
a6785352b25c: Pull complete
e9ae3c220b23: Pull complete
Digest: sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b
Status: Downloaded newer image for aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest
```

## Step 6: Delete an image<a name="cli-delete-image"></a>

If you decide that you no longer need or want an image in one of your repositories, you can delete it with the batch\-delete\-image command\. To delete an image, you must specify the repository that it is in and either a `imageTag` or `imageDigest` value for the image\. The example below deletes an image in the `hello-world` repository with the image tag `latest`\.

```
aws ecr batch-delete-image \
      --repository-name hello-world \
      --image-ids imageTag=latest
```

Output:

```
{
    "failures": [],
    "imageIds": [
        {
            "imageTag": "latest",
            "imageDigest": "sha256:215d7e4121b30157d8839e81c4e0912606fca105775bb0636b95aed25f52c89b"
        }
    ]
}
```

## Step 7: Delete a repository<a name="cli-delete-repository"></a>

If you decide that you no longer need or want an entire repository of images, you can delete the repository\. By default, you cannot delete a repository that contains images; however, the `--force` flag allows this\. To delete a repository that contains images \(and all the images within it\), run the following command\.

```
aws ecr delete-repository \
      --repository-name hello-world \
      --force
```