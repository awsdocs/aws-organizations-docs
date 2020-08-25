# SCP syntax<a name="orgs_manage_policies_scps_syntax"></a>

Service control policies \(SCPs\) use a similar syntax to that used by AWS Identity and Access Management \(IAM\) permission policies and resource\-based policies \(like Amazon S3 bucket policies\)\. For more information about IAM policies and their syntax, see [Overview of IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

An SCP is a plaintext file that is structured according to the rules of [JSON](http://json.org)\. It uses the elements that are described in this topic\.

**Note**  
All characters in your SCP count against its [maximum size](orgs_reference_limits.md#min-max-values)\. The examples in this guide show the SCPs formatted with extra white space to improve their readability\. However, to save space if your policy size approaches the maximum size, you can delete any white space, such as space characters and line breaks that are outside quotation marks\.

For general information about SCPs, see [Service control policies](orgs_manage_policies_scps.md)\.

## Elements summary<a name="scp-elements-table"></a>

The following table summarizes the policy elements that you can use in SCPs\. Some policy elements are available only in SCPs that deny actions\. The **Supported effects** column lists the effect type that you can use with each policy element in SCPs\.


| Element | Purpose | Supported effects | 
| --- | --- | --- | 
| [Version](#scp-syntax-version) | Specifies the language syntax rules to use for processing the policy\. |  `Allow`, `Deny`  | 
| [Statement](#scp-syntax-statement) | Serves as the container for policy elements\. You can have multiple statements in SCPs\. | Allow, Deny | 
| [Statement ID \(Sid\)](#scp-syntax-sid) | \(Optional\) Provides a friendly name for the statement\. | Allow, Deny | 
| [Effect](#scp-syntax-effect) | Defines whether the SCP statement [allows](orgs_getting-started_concepts.md#allowlist) or [denies](orgs_getting-started_concepts.md#denylist) principal and root access in an account\. | Allow, Deny | 
|  [Action](#scp-syntax-action)  |  Specifies AWS service and actions that the SCP allows or denies\.  |  `Allow`, `Deny`  | 
|  [NotAction](#scp-syntax-action)  |  Specifies AWS service and actions that are exempt from the SCP\. Used instead of the `Action` element\.  |  `Deny`  | 
| [Resource](#scp-syntax-resource) | Specifies the AWS resources that the SCP applies to\. | Deny | 
| [Condition](#scp-syntax-condition) | Specifies conditions for when the statement is in effect\. | Deny | 

The following sections provide more information and examples of how policy elements are used in SCPs\.

## `Version` element<a name="scp-syntax-version"></a>

Every SCP must include a `Version` element with the value `"2012-10-17"`\. This is the same version value as the most recent version of IAM permission policies\.

```
    "Version": "2012-10-17",
```

For more information, see [IAM JSON Policy Elements: Version](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_version.html) in the *IAM User Guide*\.

## `Statement` element<a name="scp-syntax-statement"></a>

An SCP consists of one or more `Statement` elements\. You can have only one `Statement` keyword in a policy, but the value can be a JSON array of statements \(surrounded by \[ \] characters\)\.

The following example shows a single statement that consists of single `Effect`, `Action`, and `Resource` elements\.

```
    "Statement": {
        "Effect": "Allow",
        "Action": "*",
        "Resource": "*"
    }
```

The following example includes two statements as an array list inside one `Statement` element\. The first statement allows all actions, while the second denies any EC2 actions\. The result is that an administrator in the account can delegate any permission *except* those from Amazon Elastic Compute Cloud \(Amazon EC2\)\.

```
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": "ec2:*",
            "Resource": "*"
        }
    ]
```

For more information, see [IAM JSON Policy Elements: Statement](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_statement.html) in the *IAM User Guide*\.

## Statement ID \(`Sid`\) element<a name="scp-syntax-sid"></a>

The `Sid` is an optional identifier that you provide for the policy statement\. You can assign a `Sid` value to each statement in a statement array\. The following example SCP shows a sample `Sid` statement\. 

```
{
    "Statement": {
        "Sid": "AllowsAllActions",
        "Effect": "Allow",
        "Action": "*",
        "Resource": "*"
    }
}
```

For more information, see [IAM JSON Policy Elements: Id](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_id.html) in the *IAM User Guide*\.

## `Effect` element<a name="scp-syntax-effect"></a>

Each statement must contain one `Effect` element\. The value can be either `Allow` or `Deny`\. It affects any actions listed in the same statement\.

For more information, see [IAM JSON Policy Elements: Effect](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_effect.html) in the *IAM User Guide*\.

### `"Effect": "Allow"`<a name="scp-syntax-effect-allow"></a>

The following example shows an SCP with a statement that contains an `Effect` element with a value of `Allow` that permits account users to perform actions for the Amazon S3 service\. This example is useful in an organization that uses the [allow list strategy](orgs_getting-started_concepts.md#allowlist_denylist) \(where the default `FullAWSAccess` policies are all detached so that permissions are implicitly denied by default\)\. The result is that the statement [allows](orgs_getting-started_concepts.md#allowlist) the Amazon S3 permissions for any attached accounts:

```
{
    "Statement": {
        "Effect": "Allow",
        "Action": "s3:*"
    }
}
```

Even though this statement uses the same `Allow` value keyword as an IAM permission policy, in an SCP it doesn't actually grant a user permission to do anything\. Instead, SCPs act as filters that specify the maximum permissions for the accounts in an organization, organizational unit \(OU\), or account\. In the preceding example, even if a user in the account had the `AdministratorAccess` managed policy attached, this SCP limits ***all*** users in affected accounts to only Amazon S3 actions\.

### `"Effect": "Deny"`<a name="scp-syntax-effect-deny"></a>

In a statement where the `Effect` element has a value of `Deny`, you can also restrict access to specific resources or define conditions for when SCPs are in effect\. 

The following shows an example of how to use a condition key in a deny statement\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Deny",
        "Action": "ec2:RunInstances",
        "Resource": "arn:aws:ec2:*:*:instance/*",
        "Condition": {
            "StringNotEquals": {
                "ec2:InstanceType": "t2.micro"
            }
        }
    }
}
```

This statement in an SCP sets a guardrail to prevent affected accounts \(where the SCP is attached to the account itself or to the organization root or OU that contains the account\), from launching Amazon EC2 instances if the Amazon EC2 instance isn't set to `t2.micro`\. Even if an IAM policy that allows this action is attached to the account, the guardrail created by the SCP prevents it\.

## `Action` and `NotAction` elements<a name="scp-syntax-action"></a>

Each statement must contain one of the following:
+ In allow and deny statements, an `Action` element\.
+ In deny statements only \(where the value of the `Effect` element is `Deny`\), an `Action` *or* `NotAction` element\.

The value for the `Action` or `NotAction` element is a list \(a JSON array\) of strings that identify AWS services and actions that are allowed or denied by the statement\.

Each string consists of the abbreviation for the service \(such as "s3", "ec2", "iam", or "organizations"\), in all lowercase, followed by a colon and then an action from that service\. The actions and notactions are case sensitive and must be typed as shown in each service's documentation\. Generally, they are all typed with each word starting with an uppercase letter and the rest lowercase\. For example: `"s3:ListAllMyBuckets"`\.

You also can use an asterisk as a wildcard to match multiple actions that share part of a name\. The value `"s3:*"` means all actions in the Amazon S3 service\. The value `"ec2:Describe*"` matches only the EC2 actions that begin with "Describe"\.

**Note**  
In an SCP, the wildcard \(\*\) character in an `Action` or `NotAction` element can be used only by itself or at the end of the string\. It can't appear at the beginning or middle of the string\. Therefore, `"servicename:action*"` is valid, but `"servicename:*action"` and `"servicename:some*action"` are both invalid in SCPs\.

For a list of all the services and the actions that they support in both AWS Organizations SCPs and IAM permission policies, see [Actions, Resources, and Condition Keys for AWS Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditions.html) in the *IAM User Guide*\.

For more information, see [IAM JSON Policy Elements: Action](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_action.html) and [IAM JSON Policy Elements: NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) in the *IAM User Guide*\.

### Example of `Action` element<a name="scp-syntax-action-example"></a>

The following example shows an SCP with a statement that permits account administrators to delegate describe, start, stop, and terminate permissions for EC2 instances in the account\. This is an example of an [allow list](orgs_getting-started_concepts.md#allowlist), and is useful when the default `Allow *` policies are ***not*** attached so that, by default, permissions are implicitly denied\. If the default `Allow *` policy is still attached to the root, OU, or account to which the following policy is attached, the policy has no effect\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
          "ec2:DescribeInstances", "ec2:DescribeImages", "ec2:DescribeKeyPairs",
          "ec2:DescribeSecurityGroups", "ec2:DescribeAvailabilityZones", "ec2:RunInstances",
          "ec2:TerminateInstances", "ec2:StopInstances", "ec2:StartInstances"
        ],
        "Resource": "*"
    }
}
```

The following example shows how you can [deny access](orgs_getting-started_concepts.md#denylist) to services that you don't want used in attached accounts\. It assumes that the default `"Allow *"` SCPs are still attached to all OUs and the root\. This example policy prevents the account administrators in attached accounts from delegating any permissions for the IAM, Amazon EC2, and Amazon RDS services\. Any action from other services can be delegated as long as there isn't another attached policy that denies them\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Deny",
        "Action": [ "iam:*", "ec2:*", "rds:*" ],
        "Resource": "*"
    }
}
```

### Example of `NotAction` element<a name="scp-syntax-notaction-example"></a>

The following example shows how you can use a `NotAction` element to exclude AWS services from the effect of the policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "LimitActionsInRegion",
      "Effect": "Deny",
      "NotAction": "iam:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": "us-west-1"
         }
       }
     }
   ]
}
```

With this statement, affected accounts are limited to taking actions in the specified AWS Region, except when using IAM actions\.

## `Resource` element<a name="scp-syntax-resource"></a>

In statements where the `Effect` element has a value of `Allow`, you can specify only "\*" in the `Resource` element of an SCP\. You can't specify individual resource Amazon Resource Names \(ARNs\)\. 

In statements where the `Effect` element has a value of `Deny`, you *can* specify individual ARNs, as shown in the following example\.

```
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAccessToAdminRole",
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

This SCP restricts IAM principals in accounts from making changes to a common administrative IAM role created in all accounts in your organization\.

For more information, see [IAM JSON Policy Elements: Resource](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_resource.html) in the *IAM User Guide*\.

## `Condition` element<a name="scp-syntax-condition"></a>

 You can specify a `Condition` element in deny statements in an SCP\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAllOutsideEU",
            "Effect": "Deny",
            "NotAction": [
                "cloudfront:*",
                "iam:*",
                "route53:*",
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

This SCP denies access to any operations outside the `eu-central-1` and `eu-west-1` Regions, except for actions in the listed services\. 

For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Unsupported elements<a name="scp-syntax-principal"></a>

The following elements aren't supported in SCPs:
+ `Principal`
+ `NotPrincipal`
+ `NotResource`