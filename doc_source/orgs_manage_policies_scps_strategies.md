# Strategies for using SCPs<a name="orgs_manage_policies_scps_strategies"></a>

You can configure the service control policies \(SCPs\) in your organization to work as either of the following:
+ A [deny list](#orgs_policies_denylist) – actions are allowed by default, and you specify what services and actions are prohibited
+ An [allow list](#orgs_policies_allowlist) – actions are prohibited by default, and you specify what services and actions are allowed

**Tip**  
You can use [service last accessed data](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) in [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) to update your SCPs to restrict access to only the AWS services that you need\. For more information, see [Viewing Organizations Service Last Accessed Data for Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-view-data-orgs.html) in the *IAM User Guide\.* 

## Using SCPs as a deny list<a name="orgs_policies_denylist"></a>

The default configuration of AWS Organizations supports using SCPs as deny lists\. Using a deny list strategy, account administrators can delegate all services and actions until you create and attach an SCP that denies a specific service or set of actions\. Deny statements require less maintenance, because you don't need to update them when AWS adds new services\. Deny statements usually use less space, thus making it easier to stay within the [maximum size for SCPs](orgs_reference_limits.md#min-max-values)\. In a statement where the `Effect` element has a value of `Deny`, you can also restrict access to specific resources, or define conditions for when SCPs are in effect\. 

To support this, AWS Organizations attaches an AWS managed SCP named [FullAWSAccess](https://console.aws.amazon.com/organizations/?#/policies/p-FullAWSAccess) to every root and OU when it's created\. This policy allows all services and actions\. It's always available for you to attach or detach from the entities in your organization as needed\. Because the policy is an AWS managed SCP, you can't modify or delete it\. The policy looks like the following\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

This policy enables account administrators to delegate permissions for any service or operation until you create and attach an SCP that denies some access\. You can attach an SCP that explicitly prohibits actions that you don't want users and roles in certain accounts to perform\.

Such a policy might look like the following example, which prevents users in the affected accounts from performing any actions for the Amazon DynamoDB service\. The organization administrator can detach the `FullAWSAccess` policy and attach this one instead\. This SCP still allows all other services and their actions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsAllActions",
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Sid": "DenyDynamoDB", 
            "Effect": "Deny",
            "Action": "dynamodb:*",
            "Resource": "*"
        }
    ]
}
```

The users in the affected accounts can't perform DynamoDB actions because the explicit `Deny` element in the second statement overrides the explicit `Allow` in the first\. You could also configure this by leaving the `FullAWSAccess` policy in place and then attaching a second policy that has only the `Deny` statement in it, as shown here\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "dynamodb:*",
            "Resource": "*"
        }
    ]
}
```

The combination of the `FullAWSAccess` policy and the `Deny` statement in the preceding DynamoDB policy that is applied to a root or OU has the same effect as the single policy that contains both statements\. All policies that apply at a specified level are combined\. Each statement, no matter which policy originated it, is evaluated according to the rules discussed earlier \(that is, an ***explicit*** `Deny` overrides an ***explicit ***`Allow`, which overrides the default ***implicit ***`Deny`\)\.

## Using SCPs as an allow list<a name="orgs_policies_allowlist"></a>

To use SCPs as an allow list, you must replace the AWS managed `FullAWSAccess` SCP with an SCP that explicitly permits only those services and actions that you want to allow\. By removing the default `FullAWSAccess` SCP, all actions for all services are now implicitly denied\. Your custom SCP then overrides the implicit `Deny` with an explicit `Allow` for only those actions that you want to permit\. For a permission to be enabled for a specified account, every SCP from the root through each OU in the direct path to the account, and even attached to the account itself, must allow that permission\.

**Notes**  
An `Allow` statement in an SCP can't have a `Resource` element with anything except a `"*"`\.
An `Allow` statement in an SCP can't have a `Condition` element at all\. 

An allow list policy might look like the following example, which enables account users to perform operations for Amazon Elastic Compute Cloud \(Amazon EC2\) and Amazon CloudWatch, but no other service\. All SCPs in parent OUs and the root also must explicitly allow these permissions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:*",
                "cloudwatch:*"
            ],
            "Resource": "*"
        }
    ]
}
```