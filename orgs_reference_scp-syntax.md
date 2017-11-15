# Service Control Policy Syntax<a name="orgs_reference_scp-syntax"></a>

Service control policies \(SCPs\) use a very similar syntax to that used by IAM permission policies and resource\-based policies \(like S3 bucket policies\)\. For more information about IAM policies and their syntax, see [Overview of IAM Policies](http://alpha-docs-aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

An SCP is a plain text file that is structured according to the rules of [JSON](http://json.org)\. It uses the elements that are described in this section\.

For general information about service control policies, see [About Service Control Policies](orgs_manage_policies_about-scps.md)\.

## `Version` Element<a name="scp-syntax-version"></a>

Every SCP must include a `Version` element with the value `"2012-10-17"`\. This is the same version value as the most recent version of IAM permission policies:

```
    "Version": "2012-10-17",
```

## `Statement` Element<a name="scp-syntax-statement"></a>

An SCP consists of one or more `Statement` elements\. You can have only one `Statement` keyword in a policy, but the value can be a JSON array of statements \(surrounded by \[ \] characters\)\.

The following example shows a single statement that consists of single `Effect`, `Action`, and `Resource` elements:

```
    "Statement": {
        "Effect": "Allow",
        "Action": "*",
        "Resource": "*"
    }
```

The following example includes two statements as an array list inside one `Statement` element\. The first statement allows all actions while the second denies any EC2 actions\. The end result is that an administrator in the account can delegate any permission *except* those from EC2:

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

## `Effect` Element<a name="scp-syntax-effect"></a>

Each statement must contain one `Effect` element\. The value can be either `Allow` or `Deny`\. It affects any actions listed in the same statement\.

The following example shows an SCP with a statement that contains an `Effect` with a value of `Allow` that permits account users to perform actions for the S3 service\. This example is useful in an organization where the default `FullAWSAccess` policies are all detached so that permissions are implicitly denied by default\. The end result is that it whitelists the S3 permissions for any attached accounts:

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "s3:*"
    }
}
```

Note that even though it uses the same `Allow` value keyword as an IAM permission policy, in an SCP it does not actually grant a user permissions to do anything\. Remember that an SCP is a filter that restricts what permissions can be used in an attached account\. In the preceding example, even if a user in the account had the `AdministratorAccess` managed policy attached, the SCP limits ***all*** users in the account to only S3 actions\.

## `Action` Element<a name="scp-syntax-action"></a>

Each statement must contain one `Action` element\. The value is a list \(a JSON array\) of strings that identify AWS services and actions that are allowed or denied by the statement\.

Each string consists of the abbreviation for the service \(such as "s3", "ec2", "iam", or "organizations"\), in all lowercase, followed by a colon and then an action from that service\. The actions are case\-sensitive, and must be typed as shown in each service's documentation\. Generally, they are all typed with each word starting with an uppercase letter and the rest lowercase\. For example: `"s3:ListAllMyBuckets"`\. You also can use an asterisk as a wildcard to match multiple actions that share part of a name\. The value `"s3:*"` means all actions in the S3 service\. The value `"ec2:Describe*"` matches only the EC2 actions that begin with "Describe"\. The wildcard can appear only by itself or at the very end of the string\. `"*"` or `"ec2:Describe*"` are both valid, while `"iam:*User"` is not valid\.

The examples in the preceding sections show simple `Action` elements that use wildcards to enable an entire service\. The following example shows an SCP with a statement that permits account administrators to delegate describe, start, stop, and terminate permissions for EC2 instances in the account\. This is another example of a whitelist, and is useful when the default `Allow *` policies are ***not*** attached so that, by default, permissions are implicitly denied\. If the default `Allow *` policy is still attached to the root, OU, or account to which the following policy is attached, then the policy has no effect:

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

The following example shows how you can blacklist services that you don't want used in attached accounts\. It assumes that the default `"Allow *"` SCPs are still attached to all OUs and the root\. This example policy prevents the account administrators in attached accounts from delegating any permissions for the IAM, Amazon EC2, or Amazon RDS services\. Any action from other services can be delegated as long as there is not another attached policy that denies them:

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

**Important**  
SCPs do ***not*** support the `NotAction` element available for IAM permission policies\.

For a list of all the services and the actions that they support in both Organizations SCPs and IAM permission policies, see [AWS Service Actions and Condition Context Keys for Use in IAM Policies](http://alpha-docs-aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditions.html) in the *IAM User Guide*\.

## `Resource` Element<a name="scp-syntax-resource"></a>

You can specify only "\*" in the `Resource` element of an SCP\. You cannot specify individual resource ARNs\.

## `Principal` Element<a name="scp-syntax-principal"></a>

You cannot specify a `Principal` element in an SCP\.