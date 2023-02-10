# General examples<a name="orgs_manage_policies_scps_examples_general"></a>

## Deny access to AWS based on the requested AWS Region<a name="example-scp-deny-region"></a>

****Examples in this category****

This SCP denies access to any operations outside of the specified Regions\. Replace `eu-central-1` and `eu-west-1` with the AWS Regions you want to use\. It provides exemptions for operations in approved global services\. This example also shows how to exempt requests made by either of two specified administrator roles\. 

**Note**  
To use the Region deny SCP with AWS Control Tower, see [ Deny access to AWS based on the requested AWS Region](#example-scp-deny-region)\.

This policy uses the `Deny` effect to deny access to all requests for operations that don't target one of the two approved regions \(`eu-central-1` and `eu-west-1`\)\. The [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element enables you to list services whose operations \(or individual operations\) are exempted from this restriction\. Because global services have endpoints that are physically hosted by the `us-east-1` Region , they must be exempted in this way\. With an SCP structured this way, requests made to global services in the `us-east-1` Region are allowed if the requested service is included in the `NotAction` element\. Any other requests to services in the `us-east-1` Region are denied by this example policy\.

**Note**  
***This example might not include all of the latest global AWS services or operations\.*** Replace the list of services and operations with the global services used by accounts in your organization\.   
You can view the[ service last accessed data in the IAM console](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) to determine what global services your organization uses\. The **Access Advisor** tab on the details page for an IAM user, group, or role displays the AWS services that have been used by that entity, sorted by most recent access\. 

**Considerations**  
AWS KMS and AWS Certificate Manager support Regional endpoints\. However, if you want to use them with a global service such as Amazon CloudFront you must include them in the global service exclusion list in the following example SCP\. A global service like Amazon CloudFront typically requires access to AWS KMS and ACM in the same region, which for a global service is the US East \(N\. Virginia\) Region \(`us-east-1`\)\.
By default, AWS STS is a global service and must be included in the global service exclusion list\. However, you can enable AWS STS to use Region endpoints instead of a single global endpoint\. If you do this, you can remove STS from the global service exemption list in the following example SCP\. For more information see [Managing AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAllOutsideEU",
            "Effect": "Deny",
            "NotAction": [
                "a4b:*",
                "acm:*",
                "aws-marketplace-management:*",
                "aws-marketplace:*",
                "aws-portal:*",
                "budgets:*",
                "ce:*",
                "chime:*",
                "cloudfront:*",
                "config:*",
                "cur:*",
                "directconnect:*",
                "ec2:DescribeRegions",
                "ec2:DescribeTransitGateways",
                "ec2:DescribeVpnGateways",
                "fms:*",
                "globalaccelerator:*",
                "health:*",
                "iam:*",
                "importexport:*",
                "kms:*",
                "mobileanalytics:*",
                "networkmanager:*",
                "organizations:*",
                "pricing:*",
                "route53:*",
                "route53domains:*",
                "s3:GetAccountPublic*",
                "s3:ListAllMyBuckets",
                "s3:PutAccountPublic*",
                "shield:*",
                "sts:*",
                "support:*",
                "supportplans:*",
                "trustedadvisor:*",
                "waf-regional:*",
                "waf:*",
                "wafv2:*",
                "wellarchitected:*"
            ],
            "Resource": "*",
            "Condition": {
                "StringNotEquals": {
                    "aws:RequestedRegion": [
                        "eu-central-1",
                        "eu-west-1"
                    ]
                },
                "ArnNotLike": {
                    "aws:PrincipalARN": [
                        "arn:aws:iam::*:role/Role1AllowedToBypassThisSCP",
                        "arn:aws:iam::*:role/Role2AllowedToBypassThisSCP"
                    ]
                }
            }
        }
    ]
}
```

## Prevent IAM users and roles from making certain changes<a name="example-scp-restricts-iam-principals"></a>

This SCP restricts IAM users and roles from making changes to the specified IAM role that you created in all accounts in your organization\.

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
        "arn:aws:iam::*:role/name-of-role-to-deny"
      ]
    }
  ]
}
```

## Prevent IAM users and roles from making specified changes, with an exception for a specified admin role<a name="example-scp-restricts-with-exception"></a>

This SCP builds on the previous example to make an exception for administrators\. It prevents IAM users and roles in affected accounts from making changes to a common administrative IAM role created in all accounts in your organization *except* for administrators using a specified role\. 

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
        "arn:aws:iam::*:role/name-of-role-to-deny"
      ],
      "Condition": {
        "StringNotLike": {
          "aws:PrincipalARN":"arn:aws:iam::*:role/name-of-admin-role-to-allow"
        }
      }
    }
  ]
}
```

## Require MFA to perform an API action<a name="example-scp-mfa"></a>

Use an SCP like the following to require that multi\-factor authentication \(MFA\) is enabled before an IAM user or role can perform an action\. In this example, the action is to stop an Amazon EC2 instance\.

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

## Block service access for the root user<a name="example-scp-root-user"></a>

The following policy restricts all access to the specified actions for the [root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) in a member account\. If you want to prevent your accounts from using root credentials in specific ways, add your own actions to this policy\.

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

## Prevent member accounts from leaving the organization<a name="example-scp-leave-org"></a>

The following policy blocks use of the `LeaveOrganization` API operation so that administrators of member accounts can't remove their accounts from the organization\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "organizations:LeaveOrganization"
            ],
            "Resource": "*"
        }
    ]
}
```
