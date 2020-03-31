# Prerequisites and permissions for managing tag policies<a name="orgs_manage_policies_tag-policies-prereqs"></a>

This page describes the prerequisites and required permissions for managing tag policies in AWS Organizations\.

**Topics**
+ [Prerequisites for managing tag policies](#tag-policies-prereqs-overview)
+ [Permissions for managing tag policies](#tag-policies-permissions-manage-policies)

## Prerequisites for managing tag policies<a name="tag-policies-prereqs-overview"></a>

Using tag policies requires the following:
+ Your organization must have [all features enabled](orgs_manage_org_support-all-features.md)\. 
+ You must be signed in to your organization's master account\. 
+ You need the permissions that are listed in [Permissions for managing tag policies](#tag-policies-permissions-manage-policies)\.

To evaluate compliance with tag policies, you use AWS Resource Groups\. For information on requirements for evaluating compliance, see [Prerequisites and Permissions](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-prereqs.html) in the *AWS Resource Groups User Guide*\.

## Permissions for managing tag policies<a name="tag-policies-permissions-manage-policies"></a>

The following example IAM policy provides permissions for managing tag policies\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ManageTagPolicies",
            "Effect": "Allow",
            "Action": [
                "organizations:ListPoliciesForTarget",
                "organizations:ListTargetsForPolicy",
                "organizations:DescribeEffectivePolicy",
                "organizations:DescribePolicy",
                "organizations:ListRoots",
                "organizations:DisableAWSServiceAccess",
                "organizations:DetachPolicy",
                "organizations:DeletePolicy",
                "organizations:DescribeAccount",
                "organizations:DisablePolicyType",
                "organizations:ListAWSServiceAccessForOrganization",
                "organizations:ListPolicies",
                "organizations:ListAccountsForParent",
                "organizations:ListAccounts",
                "organizations:EnableAWSServiceAccess",
                "organizations:ListCreateAccountStatus",
                "organizations:DescribeOrganization",
                "organizations:UpdatePolicy",
                "organizations:EnablePolicyType",
                "organizations:DescribeOrganizationalUnit",
                "organizations:AttachPolicy",
                "organizations:ListParents",
                "organizations:ListOrganizationalUnitsForParent",
                "organizations:CreatePolicy",
                "organizations:DescribeCreateAccountStatus"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information on IAM policies and permissions, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.