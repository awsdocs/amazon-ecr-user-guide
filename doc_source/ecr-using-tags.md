# Tagging an Amazon ECR repository<a name="ecr-using-tags"></a>

To help you manage your Amazon ECR repositories, you can optionally assign your own metadata to each repository in the form of *tags*\. This topic describes tags and shows you how to create them\.

**Topics**
+ [Tag basics](#tag-basics)
+ [Tagging your resources](#tag-resources)
+ [Tag restrictions](#tag-restrictions)
+ [Tagging your resources for billing](#tag-resources-for-billing)
+ [Working with tags using the console](#tag-resources-console)
+ [Working with tags using the AWS CLI or API](#tag-resources-api-sdk)

## Tag basics<a name="tag-basics"></a>

A tag is a label that you assign to an AWS resource\. Each tag consists of a *key* and an optional *value*, both of which you define\.

Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type—you can quickly identify a specific resource based on the tags you've assigned to it\. For example, you could define a set of tags for your account's Amazon ECR repositories that helps you track each repo's owner\.

We recommend that you devise a set of tag keys that meets your needs\. Using a consistent set of tag keys makes it easier for you to manage your resources\. You can search and filter the resources based on the tags you add\.

Tags don't have any semantic meaning to Amazon ECR and are interpreted strictly as a string of characters\. Also, tags are not automatically assigned to your resources\. You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the old value\. If you delete a resource, any tags for the resource are also deleted\.

You can work with tags using the AWS Management Console, the AWS CLI, and the Amazon ECR API\.

If you're using AWS Identity and Access Management \(IAM\), you can control which users in your AWS account have permission to create, edit, or delete tags\.

## Tagging your resources<a name="tag-resources"></a>

You can tag new or existing Amazon ECR repositories\.

If you're using the Amazon ECR console, you can apply tags to new resources when they are created or existing resources by using the **Tags** option on the navigation pane at any time\.

If you're using the Amazon ECR API, the AWS CLI, or an AWS SDK, you can apply tags to new repositories using the `tags` parameter on the CreateRepository API action or use the `TagResource` API action to apply tags to existing resources\. For more information, see [TagResource](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_TagResource.html)\.

Additionally, if tags cannot be applied during repository creation, we roll back the repository creation process\. This ensures that repositories are either created with tags or not created at all, and that no repositories are left untagged at any time\. By tagging repositories at the time of creation, you can eliminate the need to run custom tagging scripts after repository creation\.

## Tag restrictions<a name="tag-restrictions"></a>

The following basic restrictions apply to tags:
+ Maximum number of tags per repository – 50
+ For each repository, each tag key must be unique, and each tag key can have only one value\.
+ Maximum key length – 128 Unicode characters in UTF\-8
+ Maximum value length – 256 Unicode characters in UTF\-8
+ If your tagging schema is used across multiple services and resources, remember that other services may have restrictions on allowed characters\. Generally allowed characters are: letters, numbers, and spaces representable in UTF\-8, and the following characters: \+ \- = \. \_ : / @\.
+ Tag keys and values are case\-sensitive\.
+ Don't use the `aws:` prefix for either keys or values; it's reserved for AWS use\. You can't edit or delete tag keys or values with this prefix\. Tags with this prefix do not count against your tags per resource limit\.

## Tagging your resources for billing<a name="tag-resources-for-billing"></a>

The tags you add to your Amazon ECR repositories are helpful when reviewing cost allocation after enabling them in your Cost & Usage Report\. For more information, see [Amazon ECR usage reports](usage-reports.md)\.

To see the cost of your combined resources, you can organize your billing information based on resources that have the same tag key values\. For example, you can tag several resources with a specific application name, and then organize your billing information to see the total cost of that application across several services\. For more information about setting up a cost allocation report with tags, see [The Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html) in the *AWS Billing and Cost Management User Guide*\.

**Note**  
If you've just enabled reporting, data for the current month is available for viewing after 24 hours\.

## Working with tags using the console<a name="tag-resources-console"></a>

Using the Amazon ECR console, you can manage the tags associated with new or existing repositories\.

When you select a specific repository in the Amazon ECR console, you can view the tags by selecting **Tags** in the navigation pane\.

**To add a tag to a repository**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, select the region to use\.

1. In the navigation pane, choose **Repositories**\.

1. On the **Repositories** page, choose the repository to view\.

1. On the **Repositories : *repository\_name*** page, select **Tags** from the navigation pane\.

1. On the **Tags** page, select **Add tags**, **Add tag**\.

1. On the **Edit Tags** page, specify the key and value for each tag, and then choose **Save**\.

**To delete a tag from an individual resource**

1. Open the Amazon ECR console at [https://console\.aws\.amazon\.com/ecr/](https://console.aws.amazon.com/ecr/)\.

1. From the navigation bar, select the region to use\.

1. On the **Repositories** page, choose the repository to view\.

1. On the **Repositories : *repository\_name*** page, select **Tags** from the navigation pane\.

1. On the **Tags** page, select **Edit**\.

1. On the **Edit Tags** page, select **Remove** for each tag you want to delete, and choose **Save**\.

## Working with tags using the AWS CLI or API<a name="tag-resources-api-sdk"></a>

Use the following to add, update, list, and delete the tags for your resources\. The corresponding documentation provides examples\.


**Tagging Support for Amazon ECR Resources**  

| Task | AWS CLI | API Action | 
| --- | --- | --- | 
|  Add or overwrite one or more tags\.  |  [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/ecr/tag-resource.html)  |  [TagResource](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_TagResource.html)  | 
|  Delete one or more tags\.  |  [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/ecr/untag-resource.html)  |  [UntagResource](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_UntagResource.html)  | 

The following examples show how to manage tags using the AWS CLI\.

**Example 1: Tag an existing repository**  
The following command tags an existing repository\.

```
aws ecr tag-resource --resource-arn arn:aws:ecr:region:account_id:repository/repository_name --tags Key=stack,Value=dev
```

**Example 2: Tag an existing repository with multiple tags**  
The following command tags an existing repository\.

```
aws ecr tag-resource --resource-arn arn:aws:ecr:region:account_id:repository/repository_name --tags Key=key1,Value=value1 key=key2,value=value2 key=key3,value=value3
```

**Example 3: Untag an existing repository**  
The following command deletes a tag from an existing repository\.

```
aws ecr untag-resource --resource-arn arn:aws:ecr:region:account_id:repository/repository_name --tag-keys tag_key
```

**Example 4: List tags for a repository**  
The following command lists the tags associated with an existing repository\.

```
aws ecr list-tags-for-resource --resource-arn arn:aws:ecr:region:account_id:repository/repository_name
```

**Example 5: Create a repository and apply a tag**  
The following command creates a repository named `test-repo` and adds a tag with key `team` and value `devs`\.

```
aws ecr create-repository --repository-name test-repo --tags Key=team,Value=devs
```