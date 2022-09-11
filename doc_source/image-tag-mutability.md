# Image tag mutability<a name="image-tag-mutability"></a>

You can configure a repository to enable tag immutability to prevent image tags from being overwritten\. After the repository is configured for immutable tags, an `ImageTagAlreadyExistsException` error is returned if you attempt to push an image with a tag that is already in the repository\. When tag immutability is enabled for a repository, this affects all tags and you cannot make some tags immutable while others aren't\.

You can use the AWS Management Console and AWS CLI tools to set image tag mutability for either a new repository during creation or for an existing repository at any time\. For console steps, see [Creating a private repository](repository-create.md) and [Editing a private repository](repository-edit.md)\.

**To create a repository with immutable tags configured**

Use one of the following commands to create a new image repository with immutable tags configured\.
+ [create\-repository](https://docs.aws.amazon.com/cli/latest/reference/ecr/create-repository.html) \(AWS CLI\)

  ```
  aws ecr create-repository --repository-name name --image-tag-mutability IMMUTABLE --region us-east-2
  ```
+ [New\-ECRRepository](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECRRepository.html) \(AWS Tools for Windows PowerShell\)

  ```
  New-ECRRepository -RepositoryName name -ImageTagMutability IMMUTABLE -Region us-east-2 -Force
  ```

**To update the image tag mutability settings for an existing repository**

Use one of the following commands to update the image tag mutability settings for an existing repository\.
+ [put\-image\-tag\-mutability](https://docs.aws.amazon.com/cli/latest/reference/ecr/put-image-tag-mutability.html) \(AWS CLI\)

  ```
  aws ecr put-image-tag-mutability --repository-name name --image-tag-mutability IMMUTABLE --region us-east-2
  ```
+ [Write\-ECRImageTagMutability](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECRImageTagMutability.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECRImageTagMutability -RepositoryName name -ImageTagMutability IMMUTABLE -Region us-east-2 -Force
  ```
