# How Amazon Elastic Container Registry Works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to Amazon ECR, you should understand what IAM features are available to use with Amazon ECR\. To get a high\-level view of how Amazon ECR and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [Amazon ECR Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies)
+ [Amazon ECR Resource\-Based Policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization Based on Amazon ECR Tags](#security_iam_service-with-iam-tags)
+ [Amazon ECR IAM Roles](#security_iam_service-with-iam-roles)

## Amazon ECR Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. Amazon ECR supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\. 

Policy actions in Amazon ECR use the following prefix before the action: `ecr:`\. For example, to grant someone permission to create an Amazon ECR repository with the Amazon ECR `CreateRepository` API operation, you include the `ecr:CreateRepository` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. Amazon ECR defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "ecr:action1",
      "ecr:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "ecr:Describe*"
```



To see a list of Amazon ECR actions, see [Actions, Resources, and Condition Keys for Amazon Elastic Container Registry](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerregistry.html) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

The `Resource` element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. You specify a resource using an ARN or using the wildcard \(\*\) to indicate that the statement applies to all resources\.



An Amazon ECR repository resource has the following ARN:

```
arn:${Partition}:ecr:${Region}:${Account}:repository/${Repository-name}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `my-repo` repository in the `us-east-1` Region in your statement, use the following ARN:

```
"Resource": "arn:aws:ecr:us-east-1:123456789012:repository/my-repo"
```

To specify all repositories that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:ecr:us-east-1:123456789012:repository/*"
```

To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "resource1",
      "resource2"
```

To see a list of Amazon ECR resource types and their ARNs, see [Resources Defined by Amazon Elastic Container Registry](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerregistry.html#amazonelasticcontainerregistry-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by Amazon Elastic Container Registry](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerregistry.html#amazonelasticcontainerregistry-actions-as-permissions)\.

### Condition Keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM Policy Elements: Variables and Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

Amazon ECR defines its own set of condition keys and also supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



Most Amazon ECR actions support the `aws:ResourceTag` and `ecr:ResourceTag` condition keys\. For more information, see [Using Tag\-Based Access Control](ecr-supported-iam-actions-tagging.md)\.

To see a list of Amazon ECR condition keys, see [Condition Keys Defined by Amazon Elastic Container Registry](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerregistry.html#amazonelasticcontainerregistry-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by Amazon Elastic Container Registry](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerregistry.html#amazonelasticcontainerregistry-actions-as-permissions)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of Amazon ECR identity\-based policies, see [Amazon Elastic Container Registry Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

## Amazon ECR Resource\-Based Policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on an Amazon ECR resource and under what conditions\. Amazon ECR supports resource\-based permissions policies for Amazon ECR repositories\. Resource\-based policies let you grant usage permission to other accounts on a per\-resource basis\. You can also use a resource\-based policy to allow an AWS service to access your Amazon ECR repositories\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the [principal in a resource\-based policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, you must also grant the principal entity permission to access the resource\. Grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM Roles Differ from Resource\-based Policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)in the *IAM User Guide*\.

The Amazon ECR service supports only one type of resource\-based policy called a *repository policy*, which is attached to a *repository*\. This policy defines which principal entities \(accounts, users, roles, and federated users\) can perform actions on the repository\.

To learn how to attach a resource\-based policy to a repository, see [Repository policies](repository-policies.md)\.

### Examples<a name="security_iam_service-with-iam-resource-based-policies-examples"></a>



To view examples of Amazon ECR resource\-based policies, see [Repository policy examples](repository-policy-examples.md),

## Authorization Based on Amazon ECR Tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to Amazon ECR resources or pass tags in a request to Amazon ECR\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `ecr:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging Amazon ECR resources, see [Tagging an Amazon ECR repository](ecr-using-tags.md)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Using Tag\-Based Access Control](ecr-supported-iam-actions-tagging.md)\.

## Amazon ECR IAM Roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using Temporary Credentials with Amazon ECR<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

Amazon ECR supports using temporary credentials\. 

### Service\-Linked Roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

Amazon ECR does not support service\-linked roles\.