# Example Service Control Policies<a name="orgs_manage_policies_example-scps"></a>

**Important**  
The example service control policies \(SCPs\) displayed in this topic are for information purposes only\. Before you attempt to use them in your organization, carefully review and customize them for your unique requirements\. Remember that an SCP affects every user and role and even the root user in every account it's attached to\. Test your policies before using them in a production capacity\.
Each of the following policies is an example of a [blacklist policy](orgs_manage_policies_about-scps.md#orgs_policies_blacklist) strategy\. Blacklist policies must be attached along with other policies that allow the approved actions in the affected accounts\. For example, the default `FullAWSAccess` policy permits the use of all services in an account\. This policy is attached by default to the root, all organizational units \(OUs\), and all accounts\. It doesn't actually grant the permissions; no SCP does\. Instead, it enables administrators in that account to delegate access to those actions by attaching standard IAM permission policies to users, roles, or groups in the account\. Each of these blacklist policies then overrides any policy by blocking access to the specified services or actions\.

## Example 1: Prevent Users from Disabling AWS CloudTrail<a name="example_scp_1"></a>

This SCP prevents users or roles in any affected account from disabling or deleting a CloudTrail log, either directly as a command or through the console\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "cloudtrail:StopLogging",
        "cloudtrail:DeleteTrail"
      ],
      "Resource": "*"
    }
  ]
}
```

## Example 2: Prevent Users from Disabling Amazon CloudWatch or Altering Its Configuration<a name="example_scp_2"></a>

A lower\-level CloudWatch operator needs to monitor dashboards and alarms, but must not be able to delete or change any dashboard or alarm that senior people might put into place\. This SCP prevents users or roles in any affected account from running any of the CloudWatch commands that could delete or change your dashboards or alarms\.

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

## Example 3: Prevent Users from Deleting Amazon VPC Flow Logs<a name="example_scp_3"></a>

This SCP prevents users or roles in any affected account from deleting Amazon EC2 flow logs or CloudWatch log groups or log streams\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:DeleteFlowLogs",
        "logs:DeleteLogGroup",
        "logs:DeleteLogStream"
      ],
      "Resource": "*"
    }
  ]
 }
```

## Example 4: Prevent Users from Disabling AWS Config or Changing Its Rules<a name="example_scp_4"></a>

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

## Example 5: Prevent Any VPC That Doesn't Already Have Internet Access from Getting It<a name="example_scp_5"></a>

This SCP prevents users or roles in any affected account from changing the configuration of your Amazon EC2 virtual private clouds \(VPCs\) to grant them direct access to the internet\. It doesn't block existing direct access or any access that routes through your on\-premises network environment\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:AttachInternetGateway",
        "ec2:CreateInternetGateway",
        "ec2:AttachEgressOnlyInternetGateway",
        "ec2:CreateVpcPeeringConnection",
        "ec2:AcceptVpcPeeringConnection"
      ],
      "Resource": "*"
    }
  ]
}
```
