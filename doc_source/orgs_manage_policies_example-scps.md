# Example Service Control Policies<a name="orgs_manage_policies_example-scps"></a>

The example [service control policies \(SCPs\)](orgs_manage_policies_scp.md) displayed in this topic are for information purposes only\.

**Before Using These Examples**  
Before you attempt to use these example SCPs in your organization, do the following:  
Carefully review and customize them for your unique requirements\.
Test your policies before using them in a production capacity\. Remember that an SCP affects every user and role and even the root user in every account that it's attached to\. 

Each of the following policies is an example of a [blacklist policy](SCP_strategies.md#orgs_policies_blacklist) strategy\. Blacklist policies must be attached along with other policies that allow the approved actions in the affected accounts\. For example, the default `FullAWSAccess` policy permits the use of all services in an account\. This policy is attached by default to the root, all organizational units \(OUs\), and all accounts\. It doesn't actually grant the permissions; no SCP does\. Instead, it enables administrators in that account to delegate access to those actions by attaching standard IAM permission policies to users, roles, or groups in the account\. Each of these blacklist policies then overrides any policy by blocking access to the specified services or actions\.

**Topics**
+ [Example 1: Prevent Users from Disabling AWS CloudTrail](#example_scp_1)
+ [Example 2: Prevent Users from Disabling Amazon CloudWatch or Altering Its Configuration](#example_scp_2)
+ [Example 3: Prevent Users from Deleting Amazon VPC Flow Logs](#example_scp_3)
+ [Example 4: Prevent Users from Disabling AWS Config or Changing Its Rules](#example_scp_4)
+ [Example 5: Prevent Any VPC That Doesn't Already Have Internet Access from Getting It](#example_scp_5)
+ [Example 6: Denies Access to AWS Based on the Requested Region](#example-scp-deny-region)
+ [Example 7: Prevent IAM Principals from Making Certain Changes](#example-scp-restricts-iam-principals)
+ [Example 8: Prevent IAM Principals from Making Certain Changes, with Exceptions for Admins](#example-scp-restricts-with-exception)
+ [Example 9: Require Encryption on Amazon S3 Buckets](#example-require-encryption)
+ [Example 10: Require Amazon EC2 Instances to Use a Specific Type](#example-ec2-instances)
+ [Example 11: Require MFA to Stop an Amazon EC2 Instance](#example-ec2-mfa)

## Example 1: Prevent Users from Disabling AWS CloudTrail<a name="example_scp_1"></a>

This SCP prevents users or roles in any affected account from disabling a CloudTrail log, either directly as a command or through the console\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "cloudtrail:StopLogging",
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

## Example 6: Denies Access to AWS Based on the Requested Region<a name="example-scp-deny-region"></a>

This SCP denies access to any operations outside of the `eu-central-1` and `eu-west-1` Regions, except for actions in the listed services\. To use this SCP, replace the red italicized text in the example policy with your own information\.

This policy uses the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element with the `Deny` effect to deny access to all of the actions *not* listed in the statement\. The listed services are examples of AWS global services with a single endpoint that is physically located in the `us-east-1` Region\. Requests made to services in the `us-east-1` Region aren't denied if they're included in the `NotAction` element\. Any other requests to services in the `us-east-1` Region are denied\.

**Note**  
Not all AWS global services are shown in this example policy\. Replace the list of services in red italicized text with the global services used by accounts in your organization\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAllOutsideEU",
            "Effect": "Deny",
            "NotAction": [
               "iam:*",
               "organizations:*",
               "route53:*",
               "budgets:*",
               "waf:*",
               "cloudfront:*",
               "globalaccelerator:*",
               "importexport:*",
               "support:*"
            ],
            "Resource": "*",
            "Condition": {
                "StringNotEquals": {
                    "aws:RequestedRegion": [
                        "eu-central-1",
                        "eu-west-1"
                    ]
                }
            }
        }
    ]
}
```

## Example 7: Prevent IAM Principals from Making Certain Changes<a name="example-scp-restricts-iam-principals"></a>

This SCP restricts IAM principals in accounts from making changes to a common administrative IAM role created in all accounts in your organization\.

```
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAccessToASpecificRole",
      "Effect": "Deny",
      "Action": [
        "iam:AttachRolePolicy",
        "iam:DeleteRole",
        "iam:DeleteRolePermissionsBoundary",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:PutRolePermissionsBoundary",
        "iam:PutRolePolicy",
        "iam:UpdateAssumeRolePolicy",
        "iam:UpdateRole",
        "iam:UpdateRoleDescription"
      ],
      "Resource": [
        "arn:aws:iam::*:role/role-to-deny"
      ]
    }
  ]
}
```

## Example 8: Prevent IAM Principals from Making Certain Changes, with Exceptions for Admins<a name="example-scp-restricts-with-exception"></a>

This SCP builds on the previous example to make an exception for administrators\. It prevents IAM principals in accounts from making changes to a common administrative IAM role created in all accounts in your organization *except* for administrators using the specified role\. 

```
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAccessWithException",
      "Effect": "Deny",
      "Action": [
        "iam:AttachRolePolicy",
        "iam:DeleteRole",
        "iam:DeleteRolePermissionsBoundary",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:PutRolePermissionsBoundary",
        "iam:PutRolePolicy",
        "iam:UpdateAssumeRolePolicy",
        "iam:UpdateRole",
        "iam:UpdateRoleDescription"
      ],
      "Resource": [
        "arn:aws:iam::*:role/role-to-deny"
      ],
      "Condition": {
        "StringNotLike": {
          "aws:PrincipalARN":"arn:aws:iam::*:role/role-to-deny"
        }
      }
    }
  ]
}
```

## Example 9: Require Encryption on Amazon S3 Buckets<a name="example-require-encryption"></a>

This SCP requires that all Amazon S3 buckets use AES256 encryption\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyIncorrectEncryptionHeader",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": "AES256"
        }
      }
    },
    {
      "Sid": "DenyUnEncryptedObjectUploads",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "Bool": {
          "s3:x-amz-server-side-encryption": false
        }
      }
    }
  ]
}
```

## Example 10: Require Amazon EC2 Instances to Use a Specific Type<a name="example-ec2-instances"></a>

With this SCP, any instance launches not using the `t2.micro` instance type are denied\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireMicroInstanceType",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringNotEquals":{               	
          "ec2:InstanceType":"t2.micro"
        }
      }
    }
  ]
}
```

## Example 11: Require MFA to Stop an Amazon EC2 Instance<a name="example-ec2-mfa"></a>

Use an SCP like the following to require that multi\-factor authentication \(MFA\) is enabled before a principal or root user can stop an Amazon EC2 instance\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyStopAndTerminateWhenMFAIsNotPresent",
      "Effect": "Deny",
      "Action": [
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": "*",
      "Condition": {"BoolIfExists": {"aws:MultiFactorAuthPresent": false}}
    }
  ]
}
```