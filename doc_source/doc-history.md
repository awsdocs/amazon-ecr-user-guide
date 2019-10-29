# Document History<a name="doc-history"></a>

The following table describes the important changes to the documentation since the last release of Amazon ECR\. We also update the documentation frequently to address the feedback that you send us\.


| Change | Description | Date | 
| --- | --- | --- | 
|  Image Scanning  |  Added support for image scanning, which helps in identifying software vulnerabilities in your container images\. Amazon ECR uses the Common Vulnerabilities and Exposures \(CVEs\) database from the open source CoreOS Clair project and provides you with a list of scan findings\. For more information, see [Image Scanning](image-scanning.md)\.  |  24 Oct 2019  | 
|  VPC Endpoint Policy  |  Added support for setting an IAM policy on the Amazon ECR interface VPC endpoints\. For more information, see [Create an Endpoint Policy for your Amazon ECR VPC Endpoint](vpc-endpoints.md#ecr-vpc-endpoint-policy)\.  |  26 Sept 2019  | 
|  Image Tag Mutability  |  Added support for configuring a repository to be immutable to prevent image tags from being overwritten\. For more information, see [Image Tag Mutability](image-tag-mutability.md)\.  |  25 July 2019  | 
|  Interface VPC Endpoints \(AWS PrivateLink\)  |  Added support for configuring interface VPC endpoints powered by AWS PrivateLink\. This allows you to create a private connection between your VPC and Amazon ECR without requiring access over the Internet, through a NAT instance, a VPN connection, or AWS Direct Connect\. For more information, see [Amazon ECR Interface VPC Endpoints \(AWS PrivateLink\)](vpc-endpoints.md)\.  |  25 Jan 2019  | 
|  Resource tagging  |  Amazon ECR added support for adding metadata tags to your repositories\. For more information, see [Tagging an Amazon ECR Repository](ecr-using-tags.md)\.  |  18 Dec 2018  | 
|  Amazon ECR Name Change  | Amazon Elastic Container Registry is renamed \(previously Amazon EC2 Container Registry\)\. | 21 Nov 2017 | 
|  Lifecycle Policies  |  Amazon ECR lifecycle policies enable you to specify the lifecycle management of images in a repository\. For more information, see [Amazon ECR Lifecycle Policies](LifecyclePolicies.md)\.  | 11 Oct 2017 | 
|  Amazon ECR support for Docker image manifest 2, schema 2  |  Amazon ECR now supports Docker Image Manifest V2 Schema 2 \(used with Docker version 1\.10 and newer\)\. For more information, see [Container Image Manifest Formats](image-manifest-formats.md)\.  | 27 Jan 2017 | 
|  Amazon ECR General Availability  |  Amazon Elastic Container Registry \(Amazon ECR\) is a managed AWS Docker registry service that is secure, scalable, and reliable\.  | 21 Dec 2015 | 