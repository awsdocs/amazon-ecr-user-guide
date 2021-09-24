# Private image replication examples<a name="registry-settings-examples"></a>

The following examples show how private image replication can be used\.

## Example: Configuring cross\-Region replication to a single destination Region<a name="registry-settings-examples-crr-single"></a>

The following shows an example for configuring cross\-Region replication within a single registry\. This example assumes that your account ID is `111122223333` and that you're specifying this replication configuration in a Region other than `us-west-2`\.

```
{
    "rules": [
        {
            "destinations": [
                {
                    "region": "us-west-2",
                    "registryId": "111122223333"
                }
            ]
        }
    ]
}
```

## Example: Configuring cross\-Region replication using a repository filter<a name="registry-settings-examples-crr-filter"></a>

The following shows an example for configuring cross\-Region replication for repositories that match a prefix name value\. This example assumes your account ID is `111122223333` and that you're specifying this replication configuration in a Region other than `us-west-1` and have repositories with a prefix of `prod`\.

```
{
	"rules": [{
		"destinations": [{
			"region": "us-west-1",
			"registryId": "111122223333"
		}],
		"repositoryFilters": [{
			"filter": "prod",
			"filterType": "PREFIX_MATCH"
		}]
	}]
}
```

## Example: Configuring cross\-Region replication to multiple destination Regions<a name="registry-settings-examples-crr-multipledestinations"></a>

The following shows an example for configuring cross\-Region replication within a single registry\. This example assumes your account ID is `111122223333` and that you're specifying this replication configuration in a Region other than `us-west-1` or `us-west-2`\.

```
{
    "rules": [
        {
            "destinations": [
                {
                    "region": "us-west-1",
                    "registryId": "111122223333"
                },
                {
                    "region": "us-west-2",
                    "registryId": "111122223333"
                }
            ]
        }
    ]
}
```

## Example: Configuring cross\-account replication<a name="registry-settings-examples-crossaccount"></a>

The following shows an example for configuring cross\-account replication for your registry\. This example configures replication to the `444455556666` account and to the `us-west-2` Region\.

**Important**  
For cross\-account replication to occur, the destination account must configure a registry permissions policy to allow replication to occur\. For more information, see [Private registry permissions](registry-permissions.md)\.

```
{
    "rules": [
        {
            "destinations": [
                {
                    "region": "us-west-2",
                    "registryId": "444455556666"
                }
            ]
        }
    ]
}
```

## Example: Specifying multiple rules in a configuration<a name="registry-settings-examples-multiple-rules"></a>

The following shows an example for configuring multiple replication rules for your registry\. This example configures replication for the *111122223333* account with one rule that replicates repositories with a prefix of `prod` to the `us-west-2` Region and repositories with a prefix of `test` to the `us-east-2` Region\. A replication configuration may contain up to 10 rules, with each rule specifying up to 25 destinations\.

```
{
	"rules": [{
			"destinations": [{
				"region": "us-west-2",
				"registryId": "111122223333"
			}],
			"repositoryFilters": [{
				"filter": "prod",
				"filterType": "PREFIX_MATCH"
			}]
		},
		{
			"destinations": [{
				"region": "us-east-2",
				"registryId": "111122223333"
			}],
			"repositoryFilters": [{
				"filter": "test",
				"filterType": "PREFIX_MATCH"
			}]
		}
	]
}
```