# Encryption at rest<a name="encryption-at-rest"></a>

Amazon ECR stores images in Amazon S3 buckets that Amazon ECR manages\. By default, Amazon ECR uses server\-side encryption with Amazon S3\-managed encryption keys which encrypts your data at rest using an AES\-256 encryption algorithm\. This does not require any action on your part and is offered at no additional charge\. For more information, see [Protecting Data Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html) in the *Amazon Simple Storage Service Developer Guide*\.

For more control over the encryption for your Amazon ECR repositories, you can use server\-side encryption with customer master keys \(CMKs\) stored in AWS Key Management Service \(AWS KMS\)\. When you use AWS KMS to encrypt your data, you can either use the default AWS\-managed CMK, which is managed by Amazon ECR, or specify your own CMK \(referred to as a customer\-managed CMK\)\. For more information, see [Protecting Data Using Server\-Side Encryption with CMKs Stored in AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) in the *Amazon Simple Storage Service Developer Guide*\.

Each Amazon ECR repository has an encryption configuration, which is set when the repository is created\. You can use different encryption configurations on each repository\. For more information, see [Creating a repository](repository-create.md)\.

When a repository is created with AWS KMS encryption enabled, a CMK is used to encrypt the contents of the repository\. Moreover, Amazon ECR adds an AWS KMS grant to the CMK with the Amazon ECR repository as the grantee principal\.

The following provides a high\-level understanding of how Amazon ECR is integrated with AWS KMS to encrypt and decrypt your repositories:

1. When creating a repository, Amazon ECR sends a [DescribeKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html) call to AWS KMS to validate and retrieve the Amazon Resource Name \(ARN\) of the CMK specified in the encryption configuration\.

1. Amazon ECR sends two [CreateGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) requests to AWS KMS to create grants on the CMK to allow Amazon ECR to encrypt and decrypt data using the data key\.

1. When pushing an image, a [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKeyWithoutPlaintext.html) request is made to AWS KMS that specifies the CMK to use for encrypting the image layer and manifest\.

1. AWS KMS generates a new data key, encrypts it under the specified CMK, and sends the encrypted data key to be stored with the image layer metadata and the image manifest\.

1. When pulling an image, a [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) request is made to AWS KMS, specifying the encrypted data key\.

1. AWS KMS decrypts the encrypted data key and sends the decrypted data key to Amazon S3\.

1. The data key in used to decrypt the image layer before the image layer being pulled\.

1. When a repository is deleted, Amazon ECR sends two [RetireGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_RetireGrant.html) requests to AWS KMS to retire the grants created for the repository\.

## Considerations<a name="encryption-at-rest-considerations"></a>

The following points should be considered when using AWS KMS encryption with Amazon ECR\.
+ If you create your Amazon ECR repository with KMS encryption and you do not specify a CMK, Amazon ECR uses an AWS\-managed CMK with the alias `aws/ecr` by default\. This CMK is created in your account the first time that you create a repository with KMS encryption enabled\.
+ When you use KMS encryption with your own CMK, the key must exist in the same Region as your repository\.
+ AWS KMS enforces a limit of 500 grants per CMK\. As a result, there is a limit of 500 Amazon ECR repositories that can be encrypted per CMK\.
+ The grants that Amazon ECR creates on your behalf should not be revoked\. If you revoke the grant that gives Amazon ECR permission to use the AWS KMS keys in your account, Amazon ECR cannot access this data, encrypt new images pushed to the repository, or decrypt them when they are pulled\. When you revoke a grant for Amazon ECR, the change occurs immediately\. To revoke access rights, you should delete the repository rather than revoking the grant\. When a repository is deleted, Amazon ECR retires the grants on your behalf\.
+ There is a cost associated with using AWS KMS keys\. For more information, see [AWS Key Management Service pricing](http://aws.amazon.com/kms/pricing/)\.

## Required IAM permissions<a name="encryption-at-rest-iam"></a>

When creating or deleting an Amazon ECR repository with server\-side encryption using AWS KMS, the permissions required depend on the specific customer master key \(CMK\) you are using\. 

### Required IAM permissions when using the AWS managed CMK for Amazon ECR<a name="encryption-aws-managed-key"></a>

By default, when AWS KMS encryption is enabled for an Amazon ECR repository but no CMK is specified, the AWS\-managed CMK for Amazon ECR is used\. When the AWS\-managed CMK for Amazon ECR is used to encrypt a repository, any principal that has permission to create a repository can also enable AWS KMS encryption on the repository\. However, the IAM principal that deletes the repository must have the `kms:RetireGrant` permission\. This enables the retirement of the grants that were added to the AWS KMS key when the repository was created\.

The following example IAM policy can be added as an inline policy to a user to ensure they have the minimum permissions needed to delete a repository that has encryption enabled\. The AWS KMS key used to encrypt the repository can be specified using the resource parameter\.

```
{
    "Version": "2012-10-17",
    "Id": "ecr-kms-permissions",
    "Statement": [
        {
            "Sid": "Allow access to retire the grants associated with the key",
            "Effect": "Allow",
            "Action": [
                "kms:RetireGrant"
            ],
            "Resource": "arn:aws:kms:us-west-2:111122223333:key/b8d9ae76-080c-4043-92EXAMPLE"
        }
    ]
}
```

### Required IAM permissions when using a customer\-managed CMK<a name="encryption-customer-managed-key"></a>

When creating a repository with AWS KMS encryption enabled using a customer\-managed CMK, there are required permissions for both the CMK key policy and the IAM policy for the user or role creating the repository\.

When creating your own CMK, you can either use the default key policy AWS KMS creates or you can specify your own\. To ensure that the customer\-managed CMK remains manageable by the account owner, the key policy for the CMK should allow all AWS KMS actions for the root user of the account\. Additional scoped permissions may be added to the key policy but at minimum the root user should be given permissions to manage the CMK\. To allow the CMK to be used only for requests that originate in Amazon ECR, you can use the [kms:ViaService condition key](https://docs.aws.amazon.com/kms/latest/developerguide/policy-conditions.html#conditions-kms-via-service) with the `ecr.<region>.amazonaws.com` value\.

The following example key policy gives the AWS account \(root user\) that owns the CMK full access to the CMK\. For more information about this example key policy, see [Allows access to the AWS account and enables IAM policies](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-root-enable-iam) in the *AWS Key Management Service Developer Guide*\.

```
{
    "Version": "2012-10-17",
    "Id": "ecr-key-policy",
    "Statement": [
        {
            "Sid": "Enable IAM User Permissions",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:root"
            },
            "Action": "kms:*",
            "Resource": "*"
        }
    ]
}
```

The IAM user, IAM role, or AWS account creating your repositories must have the `kms:CreateGrant`, `kms:RetireGrant`, and `kms:DescribeKey` permission in addition to the necessary Amazon ECR permissions\.

**Note**  
The `kms:RetireGrant` permission must be added to the IAM policy of the user or role creating the repository\. The `kms:CreateGrant` and `kms:DescribeKey` permissions can be added to either the key policy for the CMK or the IAM policy of user or role creating the repository\. For more information on how AWS KMS permissions work, see [AWS KMS API permissions: Actions and resources reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.

The following example IAM policy can be added as an inline policy to a user to ensure they have the minimum permissions needed to create a repository with encryption enabled and delete the repository when they are finished with it\. The AWS KMS key used to encrypt the repository can be specified using the resource parameter\.

```
{
    "Version": "2012-10-17",
    "Id": "ecr-kms-permissions",
    "Statement": [
        {
            "Sid": "Allow access to create and retire the grants associated with the key as well as describe the key",
            "Effect": "Allow",
            "Action": [
                "kms:CreateGrant",
                "kms:RetireGrant",
                "kms:DescribeKey"
            ],
            "Resource": "arn:aws:kms:us-west-2:111122223333:key/b8d9ae76-080c-4043-92EXAMPLE"
        }
    ]
}
```

### Allow a user to list CMKs in the console when creating a repository<a name="encryption-at-rest-iam-example"></a>

When using the Amazon ECR console to create a repository, you can grant permissions to enable a user to list the customer\-managed CMKs in the Region when enabling encryption for the repository\. The following IAM policy example shows the permissions needed to list your CMKs and aliases when using the console\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": [
      "kms:ListKeys",
      "kms:ListAliases",
      "kms:DescribeKey"
    ],
    "Resource": "*"
  }
}
```

## Monitoring Amazon ECR interaction with AWS KMS<a name="encryption-at-rest-monitoring"></a>

You can use AWS CloudTrail to track the requests that Amazon ECR sends to AWS KMS on your behalf\. The log entries in the CloudTrail log contain an encryption context key to make them more easily identifiable\.

### Amazon ECR encryption context<a name="ecr-encryption-context"></a>

An *encryption context* is a set of key–value pairs that contains arbitrary nonsecret data\. When you include an encryption context in a request to encrypt data, AWS KMS cryptographically binds the encryption context to the encrypted data\. To decrypt the data, you must pass in the same encryption context\. 

In its [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html) and [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) requests to AWS KMS, Amazon ECR uses an encryption context with two name–value pairs that identify the repository and Amazon S3 bucket being used\. This is shown in the following example\. The names do not vary, but combined encryption context values will be different for each value\.

```
"encryptionContext": {
    "aws:s3:arn": "arn:aws:s3:::us-west-2-starport-manifest-bucket/EXAMPLE1-90ab-cdef-fedc-ba987BUCKET1/sha256:a7766145a775d39e53a713c75b6fd6d318740e70327aaa3ed5d09e0ef33fc3df",
    "aws:ecr:arn": "arn:aws:ecr:us-west-2:111122223333:repository/repository-name"
}
```

You can use the encryption context to identify these cryptographic operation in audit records and logs, such as [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) and Amazon CloudWatch Logs, and as a condition for authorization in policies and grants\.

The Amazon ECR encryption context consists of two name\-value pairs\.
+ **aws:s3:arn** – The first name–value pair identifies the bucket\. The key is `aws:s3:arn`\. The value is the Amazon Resource Name \(ARN\) of the Amazon S3 bucket\.

  ```
  "aws:s3:arn": "ARN of an Amazon S3 bucket"
  ```

  For example, if the ARN of the bucket is `arn:aws:s3:::us-west-2-starport-manifest-bucket/EXAMPLE1-90ab-cdef-fedc-ba987BUCKET1/sha256:a7766145a775d39e53a713c75b6fd6d318740e70327aaa3ed5d09e0ef33fc3df`, the encryption context would include the following pair\.

  ```
  "arn:aws:s3:::us-west-2-starport-manifest-bucket/EXAMPLE1-90ab-cdef-fedc-ba987BUCKET1/sha256:a7766145a775d39e53a713c75b6fd6d318740e70327aaa3ed5d09e0ef33fc3df"
  ```
+ **aws:ecr:arn** – The second name–value pair identifies the Amazon Resource Name \(ARN\) of the repository\. The key is `aws:ecr:arn`\. The value is the ARN of the repository\.

  ```
  "aws:ecr:arn": "ARN of an Amazon ECR repository"
  ```

  For example, if the ARN of the repository is `arn:aws:ecr:us-west-2:111122223333:repository/repository-name`, the encryption context would include the following pair\.

  ```
  "aws:ecr:arn": "arn:aws:ecr:us-west-2:111122223333:repository/repository-name"
  ```

## Troubleshooting<a name="encryption-at-rest-troubleshooting"></a>

When deleting an Amazon ECR repository with the console, if the repository is successfully deleted but Amazon ECR is unable to retire the grants added to your CMK for your repository, you will receive the following error\.

```
The repository [{repository-name}] has been deleted successfully but the grants created by the kmsKey [{kms_key}] failed to be retired
```

When this occurs, you can retire the AWS KMS grants for the repository yourself\.

**To retire AWS KMS grants for a repository manually**

1. List the grants for the AWS KMS key used for the repository\. The `key-id` value is included in the error you receive from the console\. You can also use the `list-keys` command to list both the AWS managed CMKs and customer\-managed CMKs in a specific Region in your account\.

   ```
   aws kms list-grants \
        --key-id b8d9ae76-080c-4043-9237-c815bfc21dfc 
        --region us-west-2
   ```

   The output include an `EncryptionContextSubset` with the Amazon Resource Name \(ARN\) of your repository\. This can be used to determine which grant added to the key is the one you want to retire\. The `GrantId` value will be used when retiring the grant in the next step\.

1. Retire each grant for the AWS KMS key added for the repository\. Replace the value for *GrantId* with the ID of the grant from the output of the previous step\.

   ```
   aws kms retire-grant \
        --key-id b8d9ae76-080c-4043-9237-c815bfc21dfc \
        --grant-id GrantId \
        --region us-west-2
   ```