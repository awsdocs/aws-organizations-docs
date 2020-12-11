# Attribute\-based access control with tags and AWS Organizations<a name="orgs_tagging_abac"></a>

*[Attribute\-based access control](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_attribute-based-access-control.html)* let you use administrator\-managed attributes such as [tags](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) attached to both AWS resources and AWS identities to control access to those resources\. For example, you can specify that a user can access a resource when both the user and the resource have the same value for a certain tag\. 

AWS Organizations taggable resources include AWS accounts, the organization's root, organizational units \(OUs\), or policies\. When you attach tags to Organizations resources, you can then use those tags to control who can access those resources\. You do this by adding `Condition` elements to your AWS Identity and Access Management \(IAM\) permissions policy statements that check whether certain tag keys and values are present before allowing the action\. This enables you to create an IAM policy that effectively says "Allow the user to manage only those OUs that have a tag with a key `X` and a value `Y`" or "Allow the user to manage only those OUs that are tagged with a key `Z` that has the same value as the user's attached tag key `Z`\." 

You can base your `Condition` tests on different types of tag references in an IAM policy\.
+ [Checking the tags that are attached to resources specified in the request](#abac-resource)
+ [Checking the tags that are attached to the IAM user or role who is making the request](#abac-prin)
+ [Check the tags that are included as parameters in the request](#abac-request)

For more information about using tags for access control in policies, see [Controlling access to and for IAM users and roles using resource tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_iam-tags.html)\. For complete syntax of IAM permission policies, see the [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html)

## Checking the tags that are attached to resources specified in the request<a name="abac-resource"></a>

When you make a request by using the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), or one of the AWS SDKs, you specify what resources you want to access with that request\. Whether you are trying to list available resources of a given type, read a resource, or write to, modify, or update a resource, you specify the resource to access as a parameter in the request\. Such requests are controlled by IAM permissions policies that you attach to your users and roles\. In these policies, you can compare the tags attached to the requested resource and choose to allow or deny access based on the keys and values of those tags\.

To check a tag that is attached to the resource, you reference the tag in a `Condition` element by prefacing the tag key name with the following string: `aws:ResourceTag/`

For example, the following sample policy allows the user or role to perform any AWS Organizations operation ***unless*** that resource has a tag with the key `department` and the value `security`\. If that key and value is present, then the policy explicitly denies the `UntagResource` operation\. 

```
{
    "Version" : "2012-10-17",
    "Statement" : [
        {
            "Effect" : "Allow",
            "Action" : "organizations:*",
            "Resource" : "*"
            
        },
        {
            "Effect" : "Deny",
            "Action" : "organizations:UntagResource",
            "Resource" : "*",
            "Condition" : {
                "StringEquals" : {
                    "aws:ResourceTag/department" : "security"
                }
            }
        }
    ]
}
```

For more information about how to use this element, see [Controlling access to resource](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_iam-tags.html#access_iam-tags_control-resources) and [aws:ResourceTag](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-resourcetag) in the *IAM User Guide*\.

## Checking the tags that are attached to the IAM user or role who is making the request<a name="abac-prin"></a>

You can control what the person making the request \(the principal\) is allowed to do based on the tags that are attached to that person's IAM user or role\. To do this, use the `aws:PrincipalTag/key-name` condition key to specify which tag and value must be attached to the calling user or role\.

The following example shows how to allow an action only when the specified tag \(`cost-center`\) has the same value on both the principal calling the operation, and the resource being accessed by the operation\. In this example, the calling user can start and stop an Amazon EC2 instance only if the instance is tagged with the same `cost-center` value as the user\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
            "ec2:startInstances",
            "ec2:stopInstances"
        ],
        "Resource": "*",
        "Condition": {"StringEquals": 
            {"ec2:ResourceTag/cost-center": "${aws:PrincipalTag/cost-center}"}}
    }
}
```

For more information about how to use this element, see [Controlling access for IAM principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_iam-tags.html#access_iam-tags_control-principals) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-principaltag](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-principaltag) in the *IAM User Guide*\.

## Check the tags that are included as parameters in the request<a name="abac-request"></a>

Several operations enable you to specify tags as part of the request\. For example, when you create a resource you can specify the tags that are attached to the new resource\. You can specify a `Condition` element that uses `aws:TagKeys` to allow or deny the operation based on whether a specific tag key, or a set of keys, is included in the request\. This comparison operator doesn't care what value the tag contains\. It only checks whether a tag with the specified key is present\. 

To check the tag key, or a list of keys, specify a `Condition` element with the following syntax:

```
"aws:TagKeys": [ "tag-key-1", "tag-key-2", ... , "tag-key-n" ]
```

You can use [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html#reference_policies_multi-key-or-value-conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html#reference_policies_multi-key-or-value-conditions) to preface the comparison operator to ensure that all of the keys in the request must match one of the keys specified in the policy\. For example, the following sample policy allows any Organizations operation only if ***all three*** of the specified tag keys are present in the request\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "organizations:*",
        "Resource": "*",
        "Condition": {
            "ForAllValues:StringEquals": {
                "aws:TagKeys": [
                    "department",
                    "costcenter",
                    "manager"
                ]
            }
        }
    }
}
```

Alternatively, you can use [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html#reference_policies_multi-key-or-value-conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html#reference_policies_multi-key-or-value-conditions) to preface a comparison operator to ensure that at least one of the keys in the request must match one of the keys specified in the policy\. For example, the following policy allows an Organizations operation only if ***at least one*** of the specified tag keys is present in the request\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "*",
        "Resource": "*",
        "Condition": {
            "ForAnyValue:StringEquals": {
                "aws:TagKeys": [
                    "stage",
                    "region",
                    "domain"
                ]
            }
        }
    }
}
```

Several operations enable you to specify tags in the request\. For example, when you create a resource you can specify the tags that are attached to the new resource\. You can compare a tag key\-value pair in the policy with a key\-value pair that is included with the request\. To do this, reference the tag in a `Condition` element by prefacing the tag key name with the following string: `aws:RequestTag/key-name` and then specify the tag value that must be present\.

For example, the following sample policy denies any request by the user or role to create an AWS account where the request is either missing the `costcenter` tag, or provides that tag with a value other than `1`, `2`, or `3`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "organizations:CreateAccount",
            "Resource": "*",
            "Condition": {
                "Null": {
                    "aws:RequestTag/costcenter": "true"
                }
            }
        },
        {
            "Effect": "Deny",
            "Action": "organizations:CreateAccount",
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringNotEquals": {
                    "aws:RequestTag/costcenter": [
                        "1",
                        "2",
                        "3"
                    ]
                }
            }
        }
    ]
}
```

For more information about how to use these elements, see [aws:TagKeys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-tagkeys) and [aws:RequestTag](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requesttag) in the *IAM User Guide*\.