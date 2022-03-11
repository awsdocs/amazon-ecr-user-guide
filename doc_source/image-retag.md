# Retagging an image<a name="image-retag"></a>

With Docker Image Manifest V2 Schema 2 images, you can use the `--image-tag` option of the put\-image command to retag an existing image\. You can retag without pulling or pushing the image with Docker\. For larger images, this process saves a considerable amount of network bandwidth and time required to retag an image\.

## To retag an image \(AWS CLI\)<a name="retag-aws-cli"></a>

**To retag an image with the AWS CLI**

1. Use the batch\-get\-image command to get the image manifest for the image to retag and write it to a file\. In this example, the manifest for an image with the tag, *latest*, in the repository, *amazonlinux*, is written to an environment variable named *MANIFEST*\.

   ```
   MANIFEST=$(aws ecr batch-get-image --repository-name amazonlinux --image-ids imageTag=latest --output json | jq --raw-output --join-output '.images[0].imageManifest')
   ```

1. Use the `--image-tag` option of the put\-image command to put the image manifest to Amazon ECR with a new tag\. In this example, the image is tagged as *2017\.03*\.
**Note**  
If the `--image-tag` option isn't available in your version of the AWS CLI, upgrade to the latest version\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr put-image --repository-name amazonlinux --image-tag 2017.03 --image-manifest "$MANIFEST"
   ```

1. Verify that your new image tag is attached to your image\. In the following output, the image has the tags `latest` and `2017.03`\.

   ```
   aws ecr describe-images --repository-name amazonlinux
   ```

   The output is as follows:

   ```
   {
       "imageDetails": [
           {
               "imageSizeInBytes": 98755613,
               "imageDigest": "sha256:8d00af8f076eb15a33019c2a3e7f1f655375681c4e5be157a26EXAMPLE",
               "imageTags": [
                   "latest",
                   "2017.03"
               ],
               "registryId": "aws_account_id",
               "repositoryName": "amazonlinux",
               "imagePushedAt": 1499287667.0
           }
       ]
   }
   ```

## To retag an image \(AWS Tools for Windows PowerShell\)<a name="retag-powershell"></a>

**To retag an image with the AWS Tools for Windows PowerShell**

1. Use the Get\-ECRImageBatch cmdlet to obtain the description of the image to retag and write it to an environment variable\. In this example, an image with the tag, *latest*, in the repository, *amazonlinux*, is written to the environment variable, *$Image*\.
**Note**  
If you don't have the Get\-ECRImageBatch cmdlet available on your system, see [Setting up the AWS Tools for Windows PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide*\.

   ```
   $Image = Get-ECRImageBatch -ImageId @{ imageTag="latest" } -RepositoryName amazonlinux
   ```

1. Write the manifest of the image to the *$Manifest* environment variable\.

   ```
   $Manifest = $Image.Images[0].ImageManifest
   ```

1. Use the `-ImageTag` option of the Write\-ECRImage cmdlet to put the image manifest to Amazon ECR with a new tag\. In this example, the image is tagged as *2017\.09*\.

   ```
   Write-ECRImage -RepositoryName amazonlinux -ImageManifest $Manifest -ImageTag 2017.09
   ```

1. Verify that your new image tag is attached to your image\. In the following output, the image has the tags `latest` and `2017.09`\.

   ```
   Get-ECRImage -RepositoryName amazonlinux
   ```

   The output is as follows:

   ```
   ImageDigest                                                             ImageTag
   -----------                                                             --------
   sha256:359b948ea8866817e94765822787cd482279eed0c17bc674a7707f4256d5d497 latest
   sha256:359b948ea8866817e94765822787cd482279eed0c17bc674a7707f4256d5d497 2017.09
   ```