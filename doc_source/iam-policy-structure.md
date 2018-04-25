# Policy Structure<a name="iam-policy-structure"></a>

The following topics explain the structure of an IAM policy\.

**Topics**
+ [Policy Syntax](#policy-syntax)
+ [Actions for Amazon ECR](#UsingWithECR_Actions)
+ [Amazon Resource Names for Amazon ECR](#ECR_ARN_Format)
+ [Condition Keys for Amazon ECR](#amazon-ecr-keys)
+ [Checking that Users Have the Required Permissions](#check-required-permissions)

## Policy Syntax<a name="policy-syntax"></a>

An IAM policy is a JSON document that consists of one or more statements\. Each statement is structured as follows:

```
{
  "Statement":[{
    "Effect":"effect",
    "Action":"action",
    "Resource":"arn",
    "Condition":{
      "condition":{
        "key":"value"
        }
      }
    }
  ]
}
```

There are various elements that make up a statement:
+ **Effect:** The *effect* can be `Allow` or `Deny`\. By default, IAM users don't have permission to use resources and API operations, so all requests are denied\. An explicit allow overrides the default\. An explicit deny overrides any allows\.
+ **Action**: The *action* is the specific API operation for which you are granting or denying permission\. To learn about specifying *action*, see [Actions for Amazon ECR](#UsingWithECR_Actions)\. 
+ **Resource**: The resource that's affected by the action\. Some Amazon ECR API operations allow you to include specific resources in your policy that can be created or modified by the operation\. To specify a resource in the statement, you need to use its Amazon Resource Name \(ARN\)\. For more information about specifying the *arn* value, see [Amazon Resource Names for Amazon ECR](#ECR_ARN_Format)\. For more information about which API operations support which ARNs, see [Supported Resource\-Level Permissions for Amazon ECR API Actions](ecr-supported-iam-actions-resources.md)\. If the API operation does not support ARNs, use the \* \(asterisk\) wildcard to specify that all resources can be affected by the operation\. 
+ **Condition**: Conditions are optional and can control when your policy will be in effect\. For more information about specifying conditions for Amazon ECR, see [Condition Keys for Amazon ECR](#amazon-ecr-keys)\.

## Actions for Amazon ECR<a name="UsingWithECR_Actions"></a>

In an IAM policy statement, you can specify any API operation from any service that supports IAM\. For Amazon ECR, use the following prefix with the name of the API operation: `ecr:`\. For example: `ecr:CreateRepository` and `ecr:DeleteRepository`\.

To specify multiple operations in a single statement, separate them with commas as follows:

```
"Action": ["ecr:action1", "ecr:action2"]
```

You can also specify multiple operations using wildcards\. For example, you can specify all operations whose name begins with the word "Delete" as follows:

```
"Action": "ecr:Delete*"
```

To specify all Amazon ECR API operations, use the \* \(asterisk\) wildcard as follows:

```
"Action": "ecr:*"
```

For a list of Amazon ECR operations, see [Actions](http://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Registry API Reference*\.

## Amazon Resource Names for Amazon ECR<a name="ECR_ARN_Format"></a>

Each IAM policy statement applies to the resources that you specify using their ARNs\. 

**Important**  
Currently, not all API actions support individual ARNs; we'll add support for additional API actions and ARNs for additional Amazon ECR resources later\. For information about which ARNs you can use with which Amazon ECR API operations, see [Supported Resource\-Level Permissions for Amazon ECR API Actions](ecr-supported-iam-actions-resources.md)\. 

An ARN has the following general syntax:

```
arn:aws:[service]:[region]:[account]:resourceType/resourcePath
```

*service*  
The service \(for example, `ecr`\)\.

*region*  
The region for the resource \(for example, `us-east-1`\)\.

*account*  
The AWS account ID, with no hyphens \(for example, `123456789012`\)\.

*resourceType*  
The type of resource \(for example, `instance`\)\.

*resourcePath*  
A path that identifies the resource\. You can use the \* \(asterisk\) wildcard in your paths\.

For example, you can indicate a specific repository \(`my-repo`\) in your statement using its ARN as follows: 

```
"Resource": "arn:aws:ecr:us-east-1:123456789012:repository/my-repo"
```

You can also specify all repositories that belong to a specific account by using the \* wildcard as follows:

```
"Resource": "arn:aws:ecr:us-east-1:123456789012:repository/*"
```

To specify all resources, or if a specific API operation does not support ARNs, use the \* wildcard in the `Resource` element as follows:

```
"Resource": "*"
```

The following table describes the ARNs for each type of resource used by the Amazon ECR API operations\. 


| Resource Type | ARN | 
| --- | --- | 
|  All Amazon ECR resources  |  arn:aws:ecr:\*  | 
|  All Amazon ECR resources owned by the specified account in the specified region  |  arn:aws:ecr:*region*:*account*:\*  | 
|  Repository  |  arn:aws:ecr:*region*:*account*:repository/*repository\-name*  | 

Many Amazon ECR API operations accept multiple resources\. To specify multiple resources in a single statement, separate their ARNs with commas, as follows:

```
"Resource": ["arn1", "arn2"]
```

For more information, see [Amazon Resource Names \(ARN\) and AWS Service Namespaces](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\. 

## Condition Keys for Amazon ECR<a name="amazon-ecr-keys"></a>

In a policy statement, you can optionally specify conditions that control when it is in effect\. Each condition contains one or more key\-value pairs\. Condition keys are not case\-sensitive\. We've defined AWS\-wide condition keys, plus additional service\-specific condition keys\.

If you specify multiple conditions, or multiple keys in a single condition, we evaluate them using a logical AND operation\. If you specify a single condition with multiple values for one key, we evaluate the condition using a logical OR operation\. For permission to be granted, all conditions must be met\.

You can also use placeholders when you specify conditions\. For more information, see [Policy Variables](http://docs.aws.amazon.com/IAM/latest/UserGuide/PolicyVariables.html) in the *IAM User Guide*\.

Amazon ECR implements the AWS\-wide condition keys \(see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AvailableKeys)\),\.

For example repository policy statements for Amazon ECR, see [Amazon ECR Repository Policies](RepositoryPolicies.md)\.

## Checking that Users Have the Required Permissions<a name="check-required-permissions"></a>

After you've created an IAM policy, we recommend that you check whether it grants users the permissions to use the particular API operations and resources they need before you put the policy into production\.

First, create an IAM user for testing purposes, and then attach the IAM policy that you created to the test user\. Then, make a request as the test user\. You can make test requests in the console or with the AWS CLI\. 

**Note**  
You can also test your policies with the [IAM Policy Simulator](https://policysim.aws.amazon.com/home/index.jsp?#)\. For more information about the policy simulator, see [Working with the IAM Policy Simulator](http://docs.aws.amazon.com/IAM/latest/UserGuide/policies_testing-policies.html) in the *IAM User Guide*\.

If the action that you are testing creates or modifies a resource, you should make the request using the `DryRun` parameter \(or run the AWS CLI command with the `--dry-run` option\)\. In this case, the call completes the authorization check, but does not complete the operation\. For example, you can check whether the user can terminate a particular instance without actually terminating it\. If the test user has the required permissions, the request returns `DryRunOperation`; otherwise, it returns `UnauthorizedOperation`\.

If the policy doesn't grant the user the permissions that you expected, or is overly permissive, you can adjust the policy as needed and retest until you get the desired results\. 

**Important**  
It can take several minutes for policy changes to propagate before they take effect\. Therefore, we recommend that you allow five minutes to pass before you test your policy updates\.

If an authorization check fails, the request returns an encoded message with diagnostic information\. You can decode the message using the `DecodeAuthorizationMessage` action\. For more information, see [DecodeAuthorizationMessage](http://docs.aws.amazon.com/STS/latest/APIReference/API_DecodeAuthorizationMessage.html) in the *AWS Security Token Service API Reference*, and [decode\-authorization\-message](http://docs.aws.amazon.com/cli/latest/reference/sts/decode-authorization-message.html) in the *AWS CLI Command Reference*\.
