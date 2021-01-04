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

## Example: Configuring cross\-Region replication to multiple destination Regions<a name="registry-settings-examples-crr-multiple"></a>

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