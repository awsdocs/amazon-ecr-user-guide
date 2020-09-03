# Logging Amazon ECR actions with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon ECR is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, a role, or an AWS service in Amazon ECR\. CloudTrail captures the following Amazon ECR actions as events:
+ All API calls, including calls from the Amazon ECR console
+ All actions taken due to the encryption settings on your repositories
+ All actions taken due to lifecycle policy rules, including both successful and unsuccessful actions

When a trail is created, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon ECR\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using this information, you can determine the request that was made to Amazon ECR, the originating IP address, who made the request, when it was made, and additional details\. 

For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon ECR information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in Amazon ECR, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Amazon ECR, create a trail\. A trail enables CloudTrail to deliver log files to an Amazon S3 bucket\. When you create a trail in the console, you can apply the trail to a single Region or to all Regions\. The trail logs events in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Creating a trail for your AWS account](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [AWS Service Integrations With CloudTrail Logs](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All Amazon ECR API actions are logged by CloudTrail and are documented in the [Amazon Elastic Container Registry API Reference](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/)\. When you perform common tasks, sections are generated in the CloudTrail log files for each API action that is part of that task\. For example, when you create a repository, `GetAuthorizationToken`, `CreateRepository` and `SetRepositoryPolicy` sections are generated in the CloudTrail log files\. When you push an image to a repository, `InitiateLayerUpload`, `UploadLayerPart`, `CompleteLayerUpload`, and `PutImage` sections are generated\. When you pull an image, `GetDownloadUrlForLayer` and `BatchGetImage` sections are generated\. For examples of these common tasks, see [CloudTrail log entry examples](#cloudtrail-examples)\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following:
+ Whether the request was made with root or IAM user credentials
+ Whether the request was made with temporary security credentials for a role or federated user
+ Whether the request was made by another AWS service

For more information, see the [CloudTrail `userIdentity` Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Understanding Amazon ECR log file entries<a name="understanding-service-name-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and other information\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

### CloudTrail log entry examples<a name="cloudtrail-examples"></a>

The following are CloudTrail log entry examples for a few common Amazon ECR tasks\.

**Note**  
These examples have been formatted for improved readability\. In a CloudTrail log file, all entries and events are concatenated into a single line\. In addition, this example has been limited to a single Amazon ECR entry\. In a real CloudTrail log file, you see entries and events from multiple AWS services\.

**Topics**
+ [Example: Create repository action](#cloudtrail-examples-create-repository)
+ [Example: AWS KMS CreateGrant API action when creating an Amazon ECR repository](#cloudtrail-examples-create-repository-kms)
+ [Example: Image push action](#cloudtrail-examples-push-image)
+ [Example: Image pull action](#cloudtrail-examples-image-pull)
+ [Example: Image lifecycle policy action](#cloudtrail-examples-lcp)

#### Example: Create repository action<a name="cloudtrail-examples-create-repository"></a>

The following example shows a CloudTrail log entry that demonstrates the `CreateRepository` action\.

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE:account_name",
        "arn": "arn:aws:sts::123456789012:user/Mary_Major",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2018-07-11T21:54:07Z"
            },
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AIDACKCEVSQ6C2EXAMPLE",
                "arn": "arn:aws:iam::123456789012:role/Admin",
                "accountId": "123456789012",
                "userName": "Admin"
            }
        }
    },
    "eventTime": "2018-07-11T22:17:43Z",
    "eventSource": "ecr.amazonaws.com",
    "eventName": "CreateRepository",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "203.0.113.12",
    "userAgent": "console.amazonaws.com",
    "requestParameters": {
        "repositoryName": "testrepo"
    },
    "responseElements": {
        "repository": {
            "repositoryArn": "arn:aws:ecr:us-east-2:123456789012:repository/testrepo",
            "repositoryName": "testrepo",
            "repositoryUri": "123456789012.dkr.ecr.us-east-2.amazonaws.com/testrepo",
            "createdAt": "Jul 11, 2018 10:17:44 PM",
            "registryId": "123456789012"
        }
    },
    "requestID": "cb8c167e-EXAMPLE",
    "eventID": "e3c6f4ce-EXAMPLE",
    "resources": [
        {
            "ARN": "arn:aws:ecr:us-east-2:123456789012:repository/testrepo",
            "accountId": "123456789012"
        }
    ],
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}
```

#### Example: AWS KMS CreateGrant API action when creating an Amazon ECR repository<a name="cloudtrail-examples-create-repository-kms"></a>

The following example shows a CloudTrail log entry that demonstrates the AWS KMS CreateGrant action when creating an Amazon ECR repository with KMS encryption enabled\. For each repository that is created with KMS encryption is enabled, you should see two CreateGrant log entries in CloudTrail\.

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAIEP6W46J43IG7LXAQ",
        "arn": "arn:aws:iam::123456789012:user/Mary_Major",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "Mary_Major",
        "sessionContext": {
            "sessionIssuer": {
                
            },
            "webIdFederationData": {
                
            },
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2020-06-10T19:22:10Z"
            }
        },
        "invokedBy": "AWS Internal"
    },
    "eventTime": "2020-06-10T19:22:10Z",
    "eventSource": "kms.amazonaws.com",
    "eventName": "CreateGrant",
    "awsRegion": "us-west-2",
    "sourceIPAddress": "203.0.113.12",
    "userAgent": "console.amazonaws.com",
    "requestParameters": {
        "keyId": "4b55e5bf-39c8-41ad-b589-18464af7758a",
        "granteePrincipal": "ecr.us-west-2.amazonaws.com",
        "operations": [
            "GenerateDataKey",
            "Decrypt"
        ],
        "retiringPrincipal": "ecr.us-west-2.amazonaws.com",
        "constraints": {
            "encryptionContextSubset": {
                "aws:ecr:arn": "arn:aws:ecr:us-west-2:123456789012:repository/testrepo"
            }
        }
    },
    "responseElements": {
        "grantId": "3636af9adfee1accb67b83941087dcd45e7fadc4e74ff0103bb338422b5055f3"
    },
    "requestID": "047b7dea-b56b-4013-87e9-a089f0f6602b",
    "eventID": "af4c9573-c56a-4886-baca-a77526544469",
    "readOnly": false,
    "resources": [
        {
            "accountId": "123456789012",
            "type": "AWS::KMS::Key",
            "ARN": "arn:aws:kms:us-west-2:123456789012:key/4b55e5bf-39c8-41ad-b589-18464af7758a"
        }
    ],
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}
```

#### Example: Image push action<a name="cloudtrail-examples-push-image"></a>

The following example shows a CloudTrail log entry that demonstrates an image push which uses the `PutImage` action\.

**Note**  
When pushing an image, you will also see `InitiateLayerUpload`, `UploadLayerPart`, and `CompleteLayerUpload` references in the CloudTrail logs\.

```
{
    "eventVersion": "1.04",
    "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDACKCEVSQ6C2EXAMPLE:account_name",
    "arn": "arn:aws:sts::123456789012:user/Mary_Major",
    "accountId": "123456789012",
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
		"userName": "Mary_Major",
		"sessionContext": {
			"attributes": {
				"mfaAuthenticated": "false",
				"creationDate": "2019-04-15T16:42:14Z"
			}
		}
	},
	"eventTime": "2019-04-15T16:45:00Z",
	"eventSource": "ecr.amazonaws.com",
	"eventName": "PutImage",
	"awsRegion": "us-east-2",
	"sourceIPAddress": "203.0.113.12",
	"userAgent": "console.amazonaws.com",
	"requestParameters": {
		"repositoryName": "testrepo",
		"imageTag": "latest",
		"registryId": "123456789012",
		"imageManifest": "{\n   \"schemaVersion\": 2,\n   \"mediaType\": \"application/vnd.docker.distribution.manifest.v2+json\",\n   \"config\": {\n      \"mediaType\": \"application/vnd.docker.container.image.v1+json\",\n      \"size\": 5543,\n      \"digest\": \"sha256:000b9b805af1cdb60628898c9f411996301a1c13afd3dbef1d8a16ac6dbf503a\"\n   },\n   \"layers\": [\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 43252507,\n         \"digest\": \"sha256:3b37166ec61459e76e33282dda08f2a9cd698ca7e3d6bc44e6a6e7580cdeff8e\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 846,\n         \"digest\": \"sha256:504facff238fde83f1ca8f9f54520b4219c5b8f80be9616ddc52d31448a044bd\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 615,\n         \"digest\": \"sha256:ebbcacd28e101968415b0c812b2d2dc60f969e36b0b08c073bf796e12b1bb449\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 850,\n         \"digest\": \"sha256:c7fb3351ecad291a88b92b600037e2435c84a347683d540042086fe72c902b8a\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 168,\n         \"digest\": \"sha256:2e3debadcbf7e542e2aefbce1b64a358b1931fb403b3e4aeca27cb4d809d56c2\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 37720774,\n         \"digest\": \"sha256:f8c9f51ad524d8ae9bf4db69cd3e720ba92373ec265f5c390ffb21bb0c277941\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 30432107,\n         \"digest\": \"sha256:813a50b13f61cf1f8d25f19fa96ad3aa5b552896c83e86ce413b48b091d7f01b\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 197,\n         \"digest\": \"sha256:7ab043301a6187ea3293d80b30ba06c7bf1a0c3cd4c43d10353b31bc0cecfe7d\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 154,\n         \"digest\": \"sha256:67012cca8f31dc3b8ee2305e7762fee20c250513effdedb38a1c37784a5a2e71\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 176,\n         \"digest\": \"sha256:3bc892145603fffc9b1c97c94e2985b4cb19ca508750b15845a5d97becbd1a0e\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 183,\n         \"digest\": \"sha256:6f1c79518f18251d35977e7e46bfa6c6b9cf50df2a79d4194941d95c54258d18\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 212,\n         \"digest\": \"sha256:b7bcfbc2e2888afebede4dd1cd5eebf029bb6315feeaf0b56e425e11a50afe42\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 212,\n         \"digest\": \"sha256:2b220f8b0f32b7c2ed8eaafe1c802633bbd94849b9ab73926f0ba46cdae91629\"\n      }\n   ]\n}"
	},
	"responseElements": {
		"image": {
			"repositoryName": "testrepo",
			"imageManifest": "{\n   \"schemaVersion\": 2,\n   \"mediaType\": \"application/vnd.docker.distribution.manifest.v2+json\",\n   \"config\": {\n      \"mediaType\": \"application/vnd.docker.container.image.v1+json\",\n      \"size\": 5543,\n      \"digest\": \"sha256:000b9b805af1cdb60628898c9f411996301a1c13afd3dbef1d8a16ac6dbf503a\"\n   },\n   \"layers\": [\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 43252507,\n         \"digest\": \"sha256:3b37166ec61459e76e33282dda08f2a9cd698ca7e3d6bc44e6a6e7580cdeff8e\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 846,\n         \"digest\": \"sha256:504facff238fde83f1ca8f9f54520b4219c5b8f80be9616ddc52d31448a044bd\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 615,\n         \"digest\": \"sha256:ebbcacd28e101968415b0c812b2d2dc60f969e36b0b08c073bf796e12b1bb449\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 850,\n         \"digest\": \"sha256:c7fb3351ecad291a88b92b600037e2435c84a347683d540042086fe72c902b8a\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 168,\n         \"digest\": \"sha256:2e3debadcbf7e542e2aefbce1b64a358b1931fb403b3e4aeca27cb4d809d56c2\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 37720774,\n         \"digest\": \"sha256:f8c9f51ad524d8ae9bf4db69cd3e720ba92373ec265f5c390ffb21bb0c277941\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 30432107,\n         \"digest\": \"sha256:813a50b13f61cf1f8d25f19fa96ad3aa5b552896c83e86ce413b48b091d7f01b\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 197,\n         \"digest\": \"sha256:7ab043301a6187ea3293d80b30ba06c7bf1a0c3cd4c43d10353b31bc0cecfe7d\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 154,\n         \"digest\": \"sha256:67012cca8f31dc3b8ee2305e7762fee20c250513effdedb38a1c37784a5a2e71\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 176,\n         \"digest\": \"sha256:3bc892145603fffc9b1c97c94e2985b4cb19ca508750b15845a5d97becbd1a0e\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 183,\n         \"digest\": \"sha256:6f1c79518f18251d35977e7e46bfa6c6b9cf50df2a79d4194941d95c54258d18\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 212,\n         \"digest\": \"sha256:b7bcfbc2e2888afebede4dd1cd5eebf029bb6315feeaf0b56e425e11a50afe42\"\n      },\n      {\n         \"mediaType\": \"application/vnd.docker.image.rootfs.diff.tar.gzip\",\n         \"size\": 212,\n         \"digest\": \"sha256:2b220f8b0f32b7c2ed8eaafe1c802633bbd94849b9ab73926f0ba46cdae91629\"\n      }\n   ]\n}",
			"registryId": "123456789012",
			"imageId": {
				"imageDigest": "sha256:98c8b060c21d9adbb6b8c41b916e95e6307102786973ab93a41e8b86d1fc6d3e",
				"imageTag": "latest"
			}
		}
	},
	"requestID": "cf044b7d-5f9d-11e9-9b2a-95983139cc57",
	"eventID": "2bfd4ee2-2178-4a82-a27d-b12939923f0f",
	"resources": [{
		"ARN": "arn:aws:ecr:us-east-2:123456789012:repository/testrepo",
		"accountId": "123456789012"
	}],
	"eventType": "AwsApiCall",
	"recipientAccountId": "123456789012"
}
```

#### Example: Image pull action<a name="cloudtrail-examples-image-pull"></a>

The following example shows a CloudTrail log entry that demonstrates an image pull which uses the `BatchGetImage` action\.

**Note**  
When pulling an image, if you don't already have the image locally, you will also see `GetDownloadUrlForLayer` references in the CloudTrail logs\.

```
{
    "eventVersion": "1.04",
    "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDACKCEVSQ6C2EXAMPLE:account_name",
    "arn": "arn:aws:sts::123456789012:user/Mary_Major",
    "accountId": "123456789012",
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
		"userName": "Mary_Major",
		"sessionContext": {
			"attributes": {
				"mfaAuthenticated": "false",
				"creationDate": "2019-04-15T16:42:14Z"
			}
		}
	},
	"eventTime": "2019-04-15T17:23:20Z",
	"eventSource": "ecr.amazonaws.com",
	"eventName": "BatchGetImage",
	"awsRegion": "us-east-2",
	"sourceIPAddress": "203.0.113.12",
	"userAgent": "console.amazonaws.com",
	"requestParameters": {
		"imageIds": [{
			"imageTag": "latest"
		}],
		"acceptedMediaTypes": [
			"application/json",
			"application/vnd.oci.image.manifest.v1+json",
			"application/vnd.oci.image.index.v1+json",
			"application/vnd.docker.distribution.manifest.v2+json",
			"application/vnd.docker.distribution.manifest.list.v2+json",
			"application/vnd.docker.distribution.manifest.v1+prettyjws"
		],
		"repositoryName": "testrepo",
		"registryId": "123456789012"
	},
	"responseElements": null,
	"requestID": "2a1b97ee-5fa3-11e9-a8cd-cd2391aeda93",
	"eventID": "c84f5880-c2f9-4585-9757-28fa5c1065df",
	"resources": [{
		"ARN": "arn:aws:ecr:us-east-2:123456789012:repository/testrepo",
		"accountId": "123456789012"
	}],
	"eventType": "AwsApiCall",
	"recipientAccountId": "123456789012"
}
```

#### Example: Image lifecycle policy action<a name="cloudtrail-examples-lcp"></a>

The following example shows a CloudTrail log entry that demonstrates when an image is expired due to a lifecycle policy rule\. This event type can be located by filtering for `PolicyExecutionEvent` for the event name field\.

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "accountId": "123456789012",
        "invokedBy": "AWS Internal"
    },
    "eventTime": "2020-03-12T20:22:12Z",
    "eventSource": "ecr.amazonaws.com",
    "eventName": "PolicyExecutionEvent",
    "awsRegion": "us-west-2",
    "sourceIPAddress": "AWS Internal",
    "userAgent": "AWS Internal",
    "requestParameters": null,
    "responseElements": null,
    "eventID": "9354dd7f-9aac-4e9d-956d-12561a4923aa",
    "readOnly": true,
    "resources": [
        {
            "ARN": "arn:aws:ecr:us-west-2:123456789012:repository/testrepo",
            "accountId": "123456789012",
            "type": "AWS::ECR::Repository"
        }
    ],
    "eventType": "AwsServiceEvent",
    "recipientAccountId": "123456789012",
    "serviceEventDetails": {
        "repositoryName": "testrepo",
        "lifecycleEventPolicy": {
            "lifecycleEventRules": [
                {
                    "rulePriority": 1,
                    "description": "remove all images > 2",
                    "lifecycleEventSelection": {
                        "tagStatus": "Any",
                        "tagPrefixList": [],
                        "countType": "Image count more than",
                        "countNumber": 2
                    },
                    "action": "expire"
                }
            ],
            "lastEvaluatedAt": 0,
            "policyVersion": 1,
            "policyId": "ceb86829-58e7-9498-920c-aa042e33037b"
        },
        "lifecycleEventImageActions": [
            {
                "lifecycleEventImage": {
                    "digest": "sha256:ddba4d27a7ffc3f86dd6c2f92041af252a1f23a8e742c90e6e1297bfa1bc0c45",
                    "tagStatus": "Tagged",
                    "tagList": [
                        "alpine"
                    ],
                    "pushedAt": 1584042813000
                },
                "rulePriority": 1
            },
            {
                "lifecycleEventImage": {
                    "digest": "sha256:6ab380c5a5acf71c1b6660d645d2cd79cc8ce91b38e0352cbf9561e050427baf",
                    "tagStatus": "Tagged",
                    "tagList": [
                        "centos"
                    ],
                    "pushedAt": 1584042842000
                },
                "rulePriority": 1
            }
        ]
    }
}
```