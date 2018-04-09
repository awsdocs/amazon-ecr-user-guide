# Amazon ECR Lifecycle Policies<a name="LifecyclePolicies"></a>

Amazon ECR lifecycle policies enable you to specify the lifecycle management of images in a repository\. A lifecycle policy is a set of one or more rules, where each rule defines an action for Amazon ECR\. The actions apply to images that contain tags prefixed with the given strings\. This allows the automation of cleaning up unused images, for example expiring images based on age or count\. You should expect that after creating a lifecycle policy the affected images will be expired within 24 hours\.

**Topics**
+ [Lifecycle Policy Template](#lifecycle_policy_syntax)
+ [Lifecycle Policy Parameters](#lifecycle_policy_parameters)
+ [Lifecycle Policy Evaluation Rules](#lp_evaluation_rules)
+ [Creating a Lifecycle Policy Preview](lpp_creation.md)
+ [Creating a Lifecycle Policy](lp_creation.md)
+ [Examples of Lifecycle Policies](lifecycle_policy_examples.md)

## Lifecycle Policy Template<a name="lifecycle_policy_syntax"></a>

The contents of your lifecycle policy is evaluated before being associated with a repository\. The following is the JSON syntax template for the lifecycle policy\. For lifecycle policy examples, see [Examples of Lifecycle Policies](lifecycle_policy_examples.md)\.

```
{
    "rules": [
        {
            "rulePriority": "integer",
            "description": string,
            "selection": {
                "tagStatus": "tagged"|"untagged",
                "tagPrefixList": list<string>,
                "countType": "imageCountMoreThan"|"sinceImagePushed",
                "countUnit": "integer",
                "countNumber": "integer"
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

**Note**  
The `tagPrefixList` parameter is only used if `tagStatus` is `tagged`\. The `countUnit` parameter is only used if `countType` is `sinceImagePushed`\. The `countNumber` parameter is only used if `countType` is set to `imageCountMoreThan`\.

## Lifecycle Policy Parameters<a name="lifecycle_policy_parameters"></a>

Lifecycle policies are split into the following parts\.

**Topics**
+ [Rule Priority](#lp_rule_priority)
+ [Description](#lp_description)
+ [Tag Status](#lp_tag_status)
+ [Tag Prefix List](#lp_tag_prefix_list)
+ [Count Type](#lp_count_type)
+ [Count Unit](#lp_count_unit)
+ [Count Number](#lp_count_number)
+ [Action](#lp_action)

### Rule Priority<a name="lp_rule_priority"></a>

`rulePriority`  
Type: integer  
Required: yes  
When you add rules to a lifecycle policy, you must give it a unique `rulePriority`\. Values do not need to be sequential across rules in a policy\.

### Description<a name="lp_description"></a>

`description`  
Type: string  
Required: no  
The description is an optional field that can be used to describe the purpose of a rule within a lifecycle policy\.

### Tag Status<a name="lp_tag_status"></a>

`tagStatus`  
Type: string  
Required: yes  
This field determines whether the lifecycle policy rule you are adding specifies a tag for an image\. Acceptable options are `tagged` or `untagged`\. If you specify `tagged`, then you must also specify a `tagPrefixList` as well\. If you specify `untagged`, then you must omit the `tagPrefixList`\.

### Tag Prefix List<a name="lp_tag_prefix_list"></a>

`tagPrefixList`  
Type: list\[string\]  
Required: yes, only if `tagStatus` is set to tagged  
This parameter is only used if you specified `"tagStatus": "tagged"`\. You must specify a comma\-separated list of image tag prefixes on which to take action with your lifecycle policy\. For example, if your images are tagged as `prod`, `prod1`, `prod2`, and so on, you would use the tag prefix `prod` to specify all of them\. If you specify multiple tags, only images with all specified tags are selected\.

### Count Type<a name="lp_count_type"></a>

`countType`  
Type: string  
Required: yes  
When you create a lifecycle policy, you specify a count type to apply to the images\. If `countType` is set to `imageCountMoreThan`, you also specify `countNumber` to create a rule that sets a limit on the number of images that exist in your repository\. If `countType` is set to `sinceImagePushed`, you also specify `countUnit` and `countNumber` to specify a time limit on the images that exist in your repository\.

### Count Unit<a name="lp_count_unit"></a>

`countUnit`  
Type: string  
Required: yes, only if `countType` is set to `sinceImagePushed`  
When you create a lifecycle policy, you specify a count unit of `days` to indicate that as the unit of time, in addition to `countNumber`, which is the number of days\. 

### Count Number<a name="lp_count_number"></a>

`countNumber`  
Type: integer  
Required: yes  
When you create a lifecycle policy, you specify a count number\. If the `countType` used is `imageCountMoreThan`, then the value is the maximum number of images that you want to retain in your repository\. If the `countType` used is `sinceImagePushed`, then the value is the maximum age limit for your images\.

### Action<a name="lp_action"></a>

`type`  
Type: string  
Required: yes  
When you create a lifecycle policy, you specify an action type\. The supported value is `expire`\.

## Lifecycle Policy Evaluation Rules<a name="lp_evaluation_rules"></a>

The lifecycle policy evaluator is responsible for parsing the plaintext JSON and applying it to the images in the specified repository\. The following rules should be noted when creating a lifecycle policy:
+ An image is expired by exactly one or zero rules\.
+ An image that matches the tagging requirements of a rule cannot be expired by a rule with a lower priority\.
+ Rules can never mark images matched by higher priority rules, but can still view them as if they haven't been expired\.
+ The set of rules must contain a unique set of tag prefixes\.
+ Only one rule is allowed to select untagged images\.
+ Expiration is always ordered by `pushed_at_time`, and always expires older images before newer ones\.
+ When using the `tagPrefixList`, an image is successfully matched if *all* of the tags in the `tagPrefixList` are matched against any of the image's tags\.
+ With `countType = imageCountMoreThan`, images are sorted from youngest to oldest based on `pushed_at_time` and then all images greater than the specified count are expired\.
+ With `countType = sinceImagePushed`, all images whose `pushed_at_time` is older than the specified number of days based on `countNumber` are expired\.