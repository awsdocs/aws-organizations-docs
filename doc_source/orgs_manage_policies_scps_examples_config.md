# Example SCPs for AWS Config<a name="orgs_manage_policies_scps_examples_config"></a>

****Examples in this category****
+ [Prevent users from disabling AWS Config or changing its rules](#example_config_1)

## Prevent users from disabling AWS Config or changing its rules<a name="example_config_1"></a>

This SCP prevents users or roles in any affected account from running AWS Config operations that could disable AWS Config or alter its rules or triggers\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "config:DeleteConfigRule",
        "config:DeleteConfigurationRecorder",
        "config:DeleteDeliveryChannel",
        "config:StopConfigurationRecorder"
      ],
      "Resource": "*"
    }
  ]
}
```