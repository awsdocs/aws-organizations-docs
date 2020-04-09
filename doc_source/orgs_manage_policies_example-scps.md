# Example service control policies<a name="orgs_manage_policies_example-scps"></a>

The example [service control policies \(SCPs\)](orgs_manage_policies_scp.md) displayed in this topic are for information purposes only\.

**Before Using These Examples**  
Before you attempt to use these example SCPs in your organization, do the following:  
Carefully review and customize them for your unique requirements\.
Test your policies before using them in a production capacity\. Remember that an SCP affects every user and role and even the root user in every account that it's attached to\. 

**Tip**  
You can use [service last accessed data](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) in [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) to update your SCPs to restrict access to only the AWS services that you need\. For more information, see [Viewing Organizations Service Last Accessed Data for Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-view-data-orgs.html) in the *IAM User Guide\.* 

Each of the following policies is an example of a [deny list policy](SCP_strategies.md#orgs_policies_denylist) strategy\. Deny list policies must be attached along with other policies that allow the approved actions in the affected accounts\. For example, the default `FullAWSAccess` policy permits the use of all services in an account\. This policy is attached by default to the root, all organizational units \(OUs\), and all accounts\. It doesn't actually grant the permissions; no SCP does\. Instead, it enables administrators in that account to delegate access to those actions by attaching standard IAM permissions policies to users, roles, or groups in the account\. Each of these deny list policies then overrides any policy by blocking access to the specified services or actions\.

**Topics**
+ [Example 1: Prevent users from disabling Amazon GuardDuty or modifying its configuration](#example_scp_1)
+ [Example 2: Prevent users from disabling Amazon CloudWatch or altering its configuration](#example_scp_2)
+ [Example 3: Prevent users from deleting Amazon VPC flow logs](#example_scp_3)
+ [Example 4: Prevent users from disabling AWS Config or changing its rules](#example_scp_4)
+ [Example 5: Prevent any VPC that doesn't already have internet access from getting it](#example_scp_5)
+ [Example 6: Denies access to AWS based on the requested Region](#example-scp-deny-region)
+ [Example 7: Prevent IAM principals from making certain changes](#example-scp-restricts-iam-principals)
+ [Example 8: Prevent IAM principals from making certain changes, with exceptions for admins](#example-scp-restricts-with-exception)
+ [Example 9: Require Amazon EC2 instances to use a specific type](#example-ec2-instances)
+ [Example 10: Require MFA to stop an Amazon EC2 instance](#example-ec2-mfa)
+ [Example 11: Restrict access to Amazon EC2 for root user](#example-ec2-root-user)
+ [Example 12: Require a tag upon resource creation](#example-require-tag-on-create)

## Example 1: Prevent users from disabling Amazon GuardDuty or modifying its configuration<a name="example_scp_1"></a>

This SCP prevents users or roles in any affected account from disabling GuardDuty or altering its configuration, either directly as a command or through the console\. It effectively enables read\-only access to the GuardDuty information and resources\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [ 
                "guardduty:AcceptInvitation",
                "guardduty:ArchiveFindings",
                "guardduty:CreateDetector",
                "guardduty:CreateFilter",
                "guardduty:CreateIPSet",
                "guardduty:CreateMembers",
                "guardduty:CreatePublishingDestination",
                "guardduty:CreateSampleFindings",
                "guardduty:CreateThreatIntelSet",
                "guardduty:DeclineInvitations",
                "guardduty:DeleteDetector",
                "guardduty:DeleteFilter",
                "guardduty:DeleteInvitations",
                "guardduty:DeleteIPSet",
                "guardduty:DeleteMembers",
                "guardduty:DeletePublishingDestination",
                "guardduty:DeleteThreatIntelSet",
                "guardduty:DisassociateFromMasterAccount",
                "guardduty:DisassociateMembers",
                "guardduty:InviteMembers",
                "guardduty:StartMonitoringMembers",
                "guardduty:StopMonitoringMembers",
                "guardduty:TagResource",
                "guardduty:UnarchiveFindings",
                "guardduty:UntagResource",
                "guardduty:UpdateDetector",
                "guardduty:UpdateFilter",
                "guardduty:UpdateFindingsFeedback",
                "guardduty:UpdateIPSet",
                "guardduty:UpdatePublishingDestination",
                "guardduty:UpdateThreatIntelSet"
            ],      
            "Resource": "*"
        }
    ]
}
```

## Example 2: Prevent users from disabling Amazon CloudWatch or altering its configuration<a name="example_scp_2"></a>

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

## Example 3: Prevent users from deleting Amazon VPC flow logs<a name="example_scp_3"></a>

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

## Example 4: Prevent users from disabling AWS Config or changing its rules<a name="example_scp_4"></a>

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

## Example 5: Prevent any VPC that doesn't already have internet access from getting it<a name="example_scp_5"></a>

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
        "ec2:CreateEgressOnlyInternetGateway",
        "ec2:CreateVpcPeeringConnection",
        "ec2:AcceptVpcPeeringConnection",
        "globalaccelerator:Create*",
        "globalaccelerator:Update*"
      ],
      "Resource": "*"
    }
  ]
}
```

## Example 6: Denies access to AWS based on the requested Region<a name="example-scp-deny-region"></a>

This SCP denies access to any operations outside of the `eu-central-1` and `eu-west-1` Regions, except for actions in the listed services\. To use this SCP, replace the red italicized text in the example policy with your own information\.

This policy uses the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element with the `Deny` effect to deny access to all of the actions *not* listed in the statement\. The listed services are examples of AWS global services with a single endpoint that is physically located in the `us-east-1` Region\. Requests made to services in the `us-east-1` Region aren't denied if they're included in the `NotAction` element\. Any other requests to services in the `us-east-1` Region are denied\.

**Notes**  
Not all AWS global services are shown in this example policy\. Replace the list of services in red italicized text with the global services used by accounts in your organization\.
This example policy blocks access to the AWS Security Token Service global endpoint \(`sts.amazonaws.com`\)\. To use AWS STS with this policy, use regional endpoints or add `"sts:*"` to the `NotAction` element\. For more information on AWS STS endpoints, see [Activating and Deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAllOutsideEU",
            "Effect": "Deny",
            "NotAction": [
               "a4b:*",
                "budgets:*",
                "ce:*",
                "chime:*",
                "cloudfront:*",
                "cur:*",
                "globalaccelerator:*",
                "health:*",
                "iam:*",
                "importexport:*",
                "mobileanalytics:*",
                "organizations:*",
                "route53:*",
                "route53domains:*",
                "shield:*",
                "support:*",
                "trustedadvisor:*",
                "waf:*",
                "wellarchitected:*"
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

## Example 7: Prevent IAM principals from making certain changes<a name="example-scp-restricts-iam-principals"></a>

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

## Example 8: Prevent IAM principals from making certain changes, with exceptions for admins<a name="example-scp-restricts-with-exception"></a>

This SCP builds on the previous example to make an exception for administrators\. It prevents IAM principals in accounts from making changes to a common administrative IAM role created in all accounts in your organization *except* for administrators using a specified role\. 

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
          "aws:PrincipalARN":"arn:aws:iam::*:role/role-to-allow"
        }
      }
    }
  ]
}
```

## Example 9: Require Amazon EC2 instances to use a specific type<a name="example-ec2-instances"></a>

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

## Example 10: Require MFA to stop an Amazon EC2 instance<a name="example-ec2-mfa"></a>

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

## Example 11: Restrict access to Amazon EC2 for root user<a name="example-ec2-root-user"></a>

The following policy restricts all access to Amazon EC2 actions for the [root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in an account\. If you want to prevent your accounts from using root credentials in specific ways, add your own actions to this policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RestrictEC2ForRoot",
      "Effect": "Deny",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ],
      "Condition": {
        "StringLike": {
          "aws:PrincipalArn": [
            "arn:aws:iam::*:root"
          ]
        }
      }
    }
  ]
}
```

## Example 12: Require a tag upon resource creation<a name="example-require-tag-on-create"></a>

The following SCP prevents account principals from creating certain resource types without the specified tags\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyCreateSecretWithNoProjectTag",
      "Effect": "Deny",
      "Action": "secretsmanager:CreateSecret",
      "Resource": "*",
      "Condition": {
        "Null": {
          "aws:RequestTag/Project": "true"
        }
      }
    },
    {
      "Sid": "DenyRunInstanceWithNoProjectTag",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": [
        "arn:aws:ec2:*:*:instance/*",
        "arn:aws:ec2:*:*:volume/*"
      ],
      "Condition": {
        "Null": {
          "aws:RequestTag/Project": "true"
        }
      }
    },
    {
      "Sid": "DenyCreateSecretWithNoCostCenterTag",
      "Effect": "Deny",
      "Action": "secretsmanager:CreateSecret",
      "Resource": "*",
      "Condition": {
        "Null": {
          "aws:RequestTag/CostCenter": "true"
        }
      }
    },
    {
      "Sid": "DenyRunInstanceWithNoCostCenterTag",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": [
        "arn:aws:ec2:*:*:instance/*",
        "arn:aws:ec2:*:*:volume/*"
      ],
      "Condition": {
        "Null": {
          "aws:RequestTag/CostCenter": "true"
        }
      }
    }
  ]
}
```

For a list of all the services and the actions that they support in both AWS Organizations SCPs and IAM permission policies, see [Actions, Resources, and Condition Keys for AWS Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditions.html) in the *IAM User Guide*\.