# Retagging an Image with the AWS CLI<a name="retag-aws-cli"></a>

With Docker Image Manifest V2 Schema 2 images, you can use the `--image-tag` option of the put\-image command to retag an existing image, without pulling or pushing the image with Docker\. For larger images, this process saves a considerable amount of network bandwidth and time required to retag an image\.

**Note**  
This procedure does not work for Windows clients because of the way the AWS CLI output text is interpreted by the shell\. To retag an image on Windows clients, see [Retagging an Image with the AWS Tools for Windows PowerShell](retag-powershell.md)\.

**To retag an image with the AWS CLI**

1. Use the batch\-get\-image command to get the image manifest for the image to retag and write it to an environment variable\. In this example, the manifest for an image with the tag, *latest*, in the repository, *amazonlinux*, is written to the environment variable, *MANIFEST*\.

   ```
   MANIFEST=$(aws ecr batch-get-image --repository-name amazonlinux --image-ids imageTag=latest --query 'images[].imageManifest' --output text)
   ```

1. Use the `--image-tag` option of the put\-image command to put the image manifest to Amazon ECR with a new tag\. In this example, the image is tagged as *2017\.03*\.
**Note**  
If the `--image-tag` option is not available in your version of the AWS CLI, upgrade to the latest version\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/) in the *AWS Command Line Interface User Guide*\.

   ```
   aws ecr put-image --repository-name amazonlinux --image-tag 2017.03 --image-manifest "$MANIFEST"
   ```

1. Verify that your new image tag is attached to your image\. In the output below, the image has the tags `latest` and `2017.03`\.

   ```
   aws ecr describe-images --repository-name amazonlinux
   ```

   Output:

   ```
   {
       "imageDetails": [
           {
               "imageSizeInBytes": 98755613,
               "imageDigest": "sha256:8d00af8f076eb15a33019c2a3e7f1f655375681c4e5be157a2685dfe6f247227",
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