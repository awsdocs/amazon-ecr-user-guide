# Retagging an Image with the AWS Tools for Windows PowerShell<a name="retag-powershell"></a>

With Docker Image Manifest V2 Schema 2 images, you can use the `-ImageTag` option of the AWS Tools for Windows PowerShell Get\-ECRImage cmdlet to retag an existing image, without pulling or pushing the image with Docker\. For larger images, this process saves a considerable amount of network bandwidth and time required to retag an image\.

**To retag an image with the AWS Tools for Windows PowerShell**

1. Use the Get\-ECRImageBatch cmdlet to get the description of the image to retag and write it to an environment variable\. In this example, an image with the tag, *latest*, in the repository, *amazonlinux*, is written to the environment variable, *$Image*\.
**Note**  
If you don't have the Get\-ECRImageBatch cmdlet available on your system, see [Setting up the AWS Tools for Windows PowerShell](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide*\.

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

1. Verify that your new image tag is attached to your image\. In the output below, the image has the tags `latest` and `2017.09`\.

   ```
   Get-ECRImage -RepositoryName amazonlinux
   ```

   Output:

   ```
   ImageDigest                                                             ImageTag
   -----------                                                             --------
   sha256:359b948ea8866817e94765822787cd482279eed0c17bc674a7707f4256d5d497 latest
   sha256:359b948ea8866817e94765822787cd482279eed0c17bc674a7707f4256d5d497 2017.09
   ```