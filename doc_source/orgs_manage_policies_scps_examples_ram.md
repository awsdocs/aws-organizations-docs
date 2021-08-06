# Example SCPs for AWS Resource Access Manager<a name="orgs_manage_policies_scps_examples_ram"></a>

****Examples in this category****
+ [Preventing external sharing](#example_ram_1)
+ [Allowing specific accounts to share only specified resource types](#example_ram_2)
+ [Prevent sharing with organizations or organizational units \(OUs\)](#example_ram_3)
+ [Allow sharing with only specified IAM users and roles](#example_ram_4)

## Preventing external sharing<a name="example_ram_1"></a>

The following example SCP prevents users from creating resource shares that allow sharing with IAM users and roles that aren't part of the organization\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "ram:CreateResourceShare",
                "ram:UpdateResourceShare"
            ],
            "Resource": "*",
            "Condition": {
                "Bool": {
                    "ram:RequestedAllowsExternalPrincipals": "true"
                }
            }
        }
    ]
}
```

## Allowing specific accounts to share only specified resource types<a name="example_ram_2"></a>

The following SCP allows accounts `111111111111` and `222222222222` to create resource shares that share prefix lists, and to associate prefix lists with existing resource shares\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "OnlyNamedAccountsCanSharePrefixLists",
            "Effect": "Deny",
            "Action": [
                "ram:AssociateResourceShare",
                "ram:CreateResourceShare"
            ],
            "Resource": "*",
            "Condition": {
                "StringNotEquals": {
                    "aws:PrincipalAccount": [
                        "111111111111",
                        "222222222222"
                    ]
                },
                "StringEquals": {
                    "ram:RequestedResourceType": "ec2:PrefixList"
                }
            }
        }
    ]
}
```

## Prevent sharing with organizations or organizational units \(OUs\)<a name="example_ram_3"></a>

The following SCP prevents users from creating resource shares that share resources with an AWS Organization or OUs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "ram:CreateResourceShare",
                "ram:AssociateResourceShare"
            ],
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringLike": {
                    "ram:Principal": [
                        "arn:aws:organizations::*:organization/*",
                        "arn:aws:organizations::*:ou/*"
                    ]
                }
            }
        }
    ]
}
```

## Allow sharing with only specified IAM users and roles<a name="example_ram_4"></a>

The following example SCP allows users to share resources with *only* organization `o-12345abcdef`, organizational unit `ou-98765fedcba`, and account `111111111111`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "ram:AssociateResourceShare",
                "ram:CreateResourceShare"
            ],
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringNotEquals": {
                    "ram:Principal": [
                        "arn:aws:organizations::123456789012:organization/o-12345abcdef",
                        "arn:aws:organizations::123456789012:ou/o-12345abcdef/ou-98765fedcba",
                        "111111111111"
                    ]
                }
            }
        }
    ]
}
```