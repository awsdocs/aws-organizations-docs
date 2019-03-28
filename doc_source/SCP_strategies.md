# Strategies for Using SCPs<a name="SCP_strategies"></a>

You can configure the SCPs in your organization to work as either of the following:
+ A [blacklist](#orgs_policies_blacklist) – actions are allowed by default, and you specify what services and actions are prohibited
+ A [whitelist](#orgs_policies_whitelist) – actions are prohibited by default, and you specify what services and actions are allowed

## Using SCPs as a Blacklist<a name="orgs_policies_blacklist"></a>

The default configuration of AWS Organizations supports using SCPs as blacklists\. Using a blacklist strategy, account administrators can delegate all services and actions until you create and attach an SCP that denies \(blacklists\) a specific service or set of actions\. Using deny statements in SCPs require less maintenance, because you don't need to update them when AWS adds new services\. Deny statements usually use less space, thus making it easier to stay within [SCP size limits](orgs_reference_limits.md#min-max-values)\. In a statement where the `Effect` element has a value of `Deny`, you can also restrict access to specific resources, or define conditions for when SCPs are in effect\. 

To support this, AWS Organizations attaches an AWS\-managed SCP named [FullAWSAccess](https://console.aws.amazon.com/organizations/?#/policies/p-FullAWSAccess) to every root and OU when it's created\. This policy allows all services and actions\. Because the policy is an AWS\-managed SCP, it can't be modified or deleted\. It's always available for you to attach or detach from the entities in your organization as needed\. The policy looks like the following\.

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

This enables account administrators to delegate permissions for any service or action until you create and attach an SCP that explicitly prohibits those actions that you don't want users and roles in certain accounts to perform\.

Such a policy might look like the following example, which prevents users in the affected accounts from performing any actions for the DynamoDB service\. The organization administrator can detach the `FullAWSAccess` policy and attach this one instead\. Note that this SCP still allows all other services and their actions\.

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

The combination of the `FullAWSAccess` policy and the `Deny` statement in the preceding DynamoDB policy that is applied to a root or OU has the exact same effect as the single policy that contains both statements\. All policies that apply at a specified level are combined together, and each statement, no matter which policy originated it, gets evaluated according to the rules discussed earlier \(that is, an ***explicit*** `Deny` overrides an ***explicit ***`Allow`, which overrides the default ***implicit ***`Deny`\)\.

## Using SCPs as a Whitelist<a name="orgs_policies_whitelist"></a>

To use SCPs as a whitelist, you must replace the AWS\-managed `FullAWSAccess` SCP with an SCP that explicitly permits only those services and actions that you want to allow\. By removing the default `FullAWSAccess` SCP, all actions for all services are now implicitly denied\. Your custom SCP then overrides the implicit `Deny` with an explicit `Allow` for only those actions that you want to permit\. Note that for a permission to be enabled for a specified account, every SCP from the root through each OU in the direct path to the account, and even attached to the account itself, must allow that permission\.

Such a whitelist policy might look like the following example, which enables account users to perform operations for Amazon EC2 and Amazon CloudWatch, but no other service\. All SCPs in parent OUs and the root also must explicitly allow these permissions\.

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