# Amazon ECR Troubleshooting<a name="troubleshooting"></a>

This chapter helps you find diagnostic information for Amazon Elastic Container Registry \(Amazon ECR\), and provides troubleshooting steps for common issues and error messages\.

**Topics**
+ [Enabling Docker Debug Output](#debug)
+ [Enabling AWS CloudTrail](#cloudtrail)
+ [Optimizing Performance for Amazon ECR](#performance)
+ [Troubleshooting Errors with Docker Commands When Using Amazon ECR](common-errors-docker.md)
+ [Troubleshooting Amazon ECR Error Messages](common-errors.md)
+ [Troubleshooting Image Scanning Issues](image-scanning-troubleshooting.md)

## Enabling Docker Debug Output<a name="debug"></a>

To begin debugging any Docker\-related issue, you should start by enabling Docker debugging output on the Docker daemon running on your host instances\. For more information about enabling Docker debugging if you are using images pulled from Amazon ECR on Amazon ECS container instances, see [Enabling Docker Debug Output](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/troubleshooting.html#docker-debug-mode) in the *Amazon Elastic Container Service Developer Guide*\. 

## Enabling AWS CloudTrail<a name="cloudtrail"></a>

 Additional information about errors returned by Amazon ECR can be discovered by enabling AWS CloudTrail, which is a service that records AWS calls for your AWS account\. CloudTrail delivers log files to an Amazon S3 bucket\. By using information collected by CloudTrail, you can determine what requests were successfully made to AWS services, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to turn it on and find your log files, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\. For more information on using CloudTrail with Amazon ECR, see [Logging Amazon ECR actions with AWS CloudTrail](logging-using-cloudtrail.md)\. 

## Optimizing Performance for Amazon ECR<a name="performance"></a>

The following section provides recommendations on settings and strategies that can be used to optimize performance when using Amazon ECR\.

Use Docker 1\.10 and above to take advantage of simultaneous layer uploads  
Docker images are composed of layers, which are intermediate build stages of the image\. Each line in a Dockerfile results in the creation of a new layer\. When you use Docker 1\.10 and above, Docker defaults to pushing as many layers as possible as simultaneous uploads to Amazon ECR, resulting in faster upload times\. 

Use a smaller base image   
The default images available through Docker Hub may contain many dependencies that your application doesn't require\. Consider using a smaller image created and maintained by others in the Docker community, or build your own base image using Docker's minimal scratch image\. For more information, see [Create a base image](https://docs.docker.com/engine/userguide/eng-image/baseimages/) in the Docker documentation\.

Place the dependencies that change the least earlier in your Dockerfile   
Docker caches layers, and that speeds up build times\. If nothing on a layer has changed since the last build, Docker uses the cached version instead of rebuilding the layer\. However, each layer is dependent on the layers that came before it\. If a layer changes, Docker recompiles not only that layer, but any layers that come after that layer as well\.   
To minimize the time required to rebuild a Dockerfile and to re\-upload layers, consider placing the dependencies that change the least frequently earlier in your Dockerfile\. Place rapidly changing dependencies \(such as your application's source code\) later in the stack\. 

Chain commands to avoid unnecessary file storage   
Intermediate files created on a layer remain a part of that layer even if they are deleted in a subsequent layer\. Consider the following example:  

```
WORKDIR /tmp
RUN wget http://example.com/software.tar.gz 
RUN wget tar -xvf software.tar.gz 
RUN mv software/binary /opt/bin/myapp
RUN rm software.tar.gz
```
In this example, the layers created by the first and second RUN commands contain the original \.tar\.gz file and all of its unzipped contents\. This is even though the \.tar\.gz file is deleted by the fourth RUN command\. These commands can be chained together into a single RUN statement to ensure that these unnecessary files aren't part of the final Docker image:  

```
WORKDIR /tmp
RUN wget http://example.com/software.tar.gz &&\
    wget tar -xvf software.tar.gz &&\
    mv software/binary /opt/bin/myapp &&\
    rm software.tar.gz
```

Use the closest regional endpoint  
You can reduce latency in pulling images from Amazon ECR by ensuring that you are using the regional endpoint closest to where your application is running\. If your application is running on an Amazon EC2 instance, you can use the following shell code to obtain the region from the Availability Zone of the instance:  

```
REGION=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone |\
     sed -n 's/\(\d*\)[a-zA-Z]*$/\1/p')
```
The region can be passed to AWS CLI commands using the \-\-region parameter, or set as the default region for a profile using the aws configure command\. You can also set the region when making calls using the AWS SDK\. For more information, see the documentation for the SDK for your specific programming language\.