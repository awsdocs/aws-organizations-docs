# Example SCPs for Amazon CloudWatch<a name="orgs_manage_policies_scps_examples_cloudwatch"></a>

****Examples in this category****
+ [Prevent users from disabling CloudWatch or altering its configuration](#example_cloudwatch_1)

## Prevent users from disabling CloudWatch or altering its configuration<a name="example_cloudwatch_1"></a>

A lower\-level CloudWatch operator needs to monitor dashboards and alarms\. However, the operator must not be able to delete or change any dashboard or alarm that senior people might put into place\. This SCP prevents users or roles in any affected account from running any of the CloudWatch commands that could delete or change your dashboards or alarms\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "cloudwatch:DeleteAlarms",
        "cloudwatch:DeleteDashboards",
        "cloudwatch:DisableAlarmActions",
        "cloudwatch:PutDashboard",
        "cloudwatch:PutMetricAlarm",
        "cloudwatch:SetAlarmState"
      ],
      "Resource": "*"
    }
  ]
}
```