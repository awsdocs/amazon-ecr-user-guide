# Examples of Lifecycle Policies<a name="lifecycle_policy_examples"></a>

The following are example lifecycle policies, showing the syntax\.


+ [Filtering on Image Age](#lifecycle_policy_example_age)
+ [Filtering on Image Count](#lifecycle_policy_example_number)
+ [Filtering on Multiple Rules](#lp_example_multiple)
+ [Filtering on Multiple Tags in a Single Rule](#lp_example_difftype)

## Filtering on Image Age<a name="lifecycle_policy_example_age"></a>

The following shows the lifecycle policy syntax for a policy that expires untagged images older than `14` days:

```
{
    "rules": [
        {
            "rulePriority": 1,
            "description": "Expire images older than 14 days",
            "selection": {
                "tagStatus": "untagged",
                "countType": "sinceImagePushed",
                "countUnit": "days",
                "countNumber": 14
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

## Filtering on Image Count<a name="lifecycle_policy_example_number"></a>

The following shows the lifecycle policy syntax for a policy that keeps only one untagged image and expires all others:

```
{
    "rules": [
        {
            "rulePriority": 1,
            "description": "Keep only one untagged image, expire all others",
            "selection": {
                "tagStatus": "untagged",
                "countType": "imageCountMoreThan",
                "countNumber": 1
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

## Filtering on Multiple Rules<a name="lp_example_multiple"></a>

The following examples use multiple rules in a lifecycle policy\. An example repository and lifecycle policy are given along with an explanation of the outcome\.

### Example A<a name="lp_example_multiple_a"></a>

Repository contents:

+ Image A, Taglist: \["beta\-1", "prod\-1"\], Pushed: 10 days ago

+ Image B, Taglist: \["beta\-2", "prod\-2"\], Pushed: 9 days ago

+ Image C, Taglist: \["beta\-3"\], Pushed: 8 days ago

Lifecycle policy text:

```
{
    "rules": [
        {
            "rulePriority": 1
            "description": "Rule 1",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["prod"],
                "countType": "imageCountMoreThan",
                "countNumber": 1
            },
            "action": {
                "type": "expire"
            }
        },
        {
            "rulePriority": 2
            "description": "Rule 2",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["beta"],
                "countType": "imageCountMoreThan",
                "countNumber": 1
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

The logic of this lifecycle policy would be:

+ Rule 1 identifies images tagged with prefix `prod`\. It should mark images, starting with the oldest, until there is one or fewer images remaining that match\. It marks Image A for expiration\.

+ Rule 2 identifies images tagged with prefix `beta`\. It should mark images, starting with the oldest, until there is one or fewer images remaining that match\. It marks both Image A and Image B for expiration\. However, Image A has already been seen by Rule 1 and if Image B were expired it would violate Rule 1 and thus is skipped\.

+ Result: Image A is expired\.

### Example B<a name="lp_example_multiple_b"></a>

This is the same repository as the previous example but the rule priority order is changed to illustrate the outcome\.

Repository contents:

+ Image A, Taglist: \["beta\-1", "prod\-1"\], Pushed: 10 days ago

+ Image B, Taglist: \["beta\-2", "prod\-2"\], Pushed: 9 days ago

+ Image C, Taglist: \["beta\-3"\], Pushed: 8 days ago

Lifecycle policy text:

```
{
    "rules": [
        {
            "rulePriority": 1
            "description": "Rule 1",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["beta"],
                "countType": "imageCountMoreThan",
                "countNumber": 1
            },
            "action": {
                "type": "expire"
            }
        }
        {
            "rulePriority": 2
            "description": "Rule 2",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["prod"],
                "countType": "imageCountMoreThan",
                "countNumber": 1
            },
            "action": {
                "type": "expire"
            }
        },
    ]
}
```

The logic of this lifecycle policy would be:

+ Rule 1 identifies images tagged with `beta`\. It should mark images, starting with the oldest, until there is one or fewer images remaining that match\. It sees all three images and would mark Image A and Image B for expiration\.

+ Rule 2 identifies images tagged with `prod`\. It should mark images, starting with the oldest, until there is one or fewer images remaining that match\. It would see no images because all available images were already seen by Rule 1 and thus would mark no additional images\.

+ Result: Images A and B are expired\.

## Filtering on Multiple Tags in a Single Rule<a name="lp_example_difftype"></a>

The following lifecycle policy examples specify multiple tag prefixes in a single rule\.

### Example A<a name="lp_example_difftype_a"></a>

When multiple tag prefixes are specified on a single rule, images must match all listed tag prefixes\.

Repository contents:

+ Image A, Taglist: \["alpha\-1"\], Pushed: 12 days ago

+ Image B, Taglist: \["beta\-1"\], Pushed: 11 days ago

+ Image C, Taglist: \["alpha\-2", "beta\-2"\], Pushed: 10 days ago

+ Image D, Taglist: \["alpha\-3"\], Pushed: 4 days ago

+ Image E, Taglist: \["beta\-3"\], Pushed: 3 days ago

+ Image F, Taglist: \["alpha\-4", "beta\-4"\], Pushed: 2 days ago

```
{
    "rules": [
        {
            "rulePriority": 1
            "description": "Rule 1",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["alpha", "beta"],
                "countType": "sinceImagePushed",
                "countNumber": 5,
                "countUnit": "days"
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

The logic of this lifecycle policy would be:

+ Rule 1 identifies images tagged with `alpha` and `beta`\. It sees images C and F\. It should mark images that are older than five days, which would be Image C\.

+ Result: Image C is expired\.

### Example B<a name="lp_example_difftype_b"></a>

The following example illustrates that tags are not exclusive\.

Repository contents:

+ Image A, Taglist: \["alpha\-1", "beta\-1", "gamma\-1"\], Pushed: 10 days ago

+ Image B, Taglist: \["alpha\-2", "beta\-2"\], Pushed: 9 days ago

+ Image C, Taglist: \["alpha\-3", "beta\-3", "gamma\-2"\], Pushed: 8 days ago

```
{
    "rules": [
        {
            "rulePriority": 1
            "description": "Rule 1",
            "selection": {
                "tagStatus": "tagged",
                "tagPrefixList": ["alpha", "beta"],
                "countType": "imageCountMoreThan",
                "countNumber": 1
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

The logic of this lifecycle policy would be:

+ Rule 1 identifies images tagged with `alpha` and `beta`\. It sees all images\. It should mark images, starting with the oldest, until there is one or fewer images remaining that match\. It marks image A and B for expiration\.

+ Result: Images A and B are expired\.