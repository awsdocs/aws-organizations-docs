# Example SCPs for tagging resources<a name="orgs_manage_policies_scps_examples_tagging"></a>

****Examples in this category****
+ [Require a tag on specified created resources](#example-require-tag-on-create)
+ [Prevent tags from being modified except by authorized principals](#example-require-restrict-tag-mods-to-admin)

## Require a tag on specified created resources<a name="example-require-tag-on-create"></a>

The following SCP prevents IAM users and roles in the affected accounts from creating certain resource types if the request doesn't include the specified tags\. 

**Important**  
Remember to test Deny\-based policies with the services you use in your environment\. The following example is a simple block of creating untagged secrets or running untagged Amazon EC2 instances, and doesn't include any exceptions\.  
The following example policy is not compatible with AWS CloudFormation as written, because that service creates a secret and then tags it as two separate steps\. This example policy effectively blocks AWS CloudFormation from creating a secret as part of a stack, because such an action would result, however briefly, in a secret that is not tagged as required\.

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

For a list of all the services and the actions that they support in both AWS Organizations SCPs and IAM permission policies, see [Actions, Resources, and Condition Keys for AWS Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actions-resources-contextkeys.html) in the *IAM User Guide*\.

## Prevent tags from being modified except by authorized principals<a name="example-require-restrict-tag-mods-to-admin"></a>

The following SCP shows how a policy can allow only authorized principals to modify the tags attached to your resources\. This is an important part of using attribute\-based access control \(ABAC\) as part of your AWS cloud security strategy\. The policy allows a caller to modify the tags on only those resources where the authorization tag \(in this example, `access-project`\) exactly matches the same authorization tag attached to the user or role making the request\. The policy also prevents the authorized user from changing the *value* of the tag that is used for authorization\. The calling principal must have the authorization tag to make any changes at all\.

This policy only blocks unauthorized users from changing tags\. An authorized user who isn't blocked by this policy must still have a separate IAM policy that explicitly grants the `Allow` permission on the relevant tagging APIs\. As an example, if your user has an administrator policy with `Allow */*` \(allow all services and all operations\), then the combination results in the administrator user being allowed to change *only* those tags that have an authorization tag value that matches the authorization tag value attached to the user's principal\. This is because the explicit `Deny` in the this policy overrides the explicit `Allow` in the administrator policy\.

**Important**  
**This is not a complete policy solution and should not be used as shown here\.** This example is intended only to illustrate part of an ABAC strategy and needs to be customized and tested for production environments\.  
For the complete policy with a detailed analysis of how it works, see [Securing resource tags used for authorization using a service control policy in AWS Organizations](http://aws.amazon.com/blogs/security/securing-resource-tags-used-for-authorization-using-service-control-policy-in-aws-organizations/)  
Remember to test Deny\-based policies with the services you use in your environment\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyModifyTagsIfResAuthzTagAndPrinTagDontMatch",
            "Effect": "Deny",
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringNotEquals": {
                    "ec2:ResourceTag/access-project": "${aws:PrincipalTag/access-project}",
                    "aws:PrincipalArn": "arn:aws:iam::123456789012:role/org-admins/iam-admin"
                },
                "Null": {
                    "ec2:ResourceTag/access-project": false
                }
            }
        },
        {
            "Sid": "DenyModifyResAuthzTagIfPrinTagDontMatch",
            "Effect": "Deny",
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringNotEquals": {
                    "aws:RequestTag/access-project": "${aws:PrincipalTag/access-project}",
                    "aws:PrincipalArn": "arn:aws:iam::123456789012:role/org-admins/iam-admin"
                },
                "ForAnyValue:StringEquals": {
                    "aws:TagKeys": [
                        "access-project"
                    ]   
                }   
            }
        },
        {       
            "Sid": "DenyModifyTagsIfPrinTagNotExists",
            "Effect": "Deny", 
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags"
            ],      
            "Resource": [
                "*"     
            ],      
            "Condition": {
                "StringNotEquals": {
                    "aws:PrincipalArn": "arn:aws:iam::123456789012:role/org-admins/iam-admin"
                },      
                "Null": {
                    "aws:PrincipalTag/access-project": true
                }       
            }       
        }
    ]
}
```