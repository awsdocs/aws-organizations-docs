# Prerequisites and permissions for managing backup policies<a name="orgs_manage_policies_backup_prereqs"></a>

This page describes the prerequisites and required permissions to manage backup policies in AWS Organizations\.

**Topics**
+ [Prerequisites for managing backup policies](#backup-policies-prereqs-overview)
+ [Permissions for managing backup policies](#backup-policies-permissions-manage-policies)

## Prerequisites for managing backup policies<a name="backup-policies-prereqs-overview"></a>

To manage backup policies in an organization requires the following:
+ Your organization must have [all features enabled](orgs_manage_org_support-all-features.md)\. 
+ You must be signed in to your organization's master account\. 
+ Your AWS Identity and Access Management \(IAM\) user or role must have the permissions that are listed in the following section\.

## Permissions for managing backup policies<a name="backup-policies-permissions-manage-policies"></a>

The following example IAM policy provides permissions to manage all aspects of backup policies in an organization\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ManageBackupPolicies",
            "Effect": "Allow",
            "Action": [
                "organizations:AttachPolicy",
                "organizations:CreatePolicy",
                "organizations:DeletePolicy",
                "organizations:DescribeAccount",
                "organizations:DescribeCreateAccountStatus"
                "organizations:DescribeEffectivePolicy",
                "organizations:DescribeOrganization",
                "organizations:DescribeOrganizationalUnit",
                "organizations:DescribePolicy",
                "organizations:DetachPolicy",
                "organizations:DisableAWSServiceAccess",
                "organizations:DisablePolicyType",
                "organizations:EnableAWSServiceAccess",
                "organizations:EnablePolicyType",
                "organizations:ListAccounts",
                "organizations:ListAccountsForParent",
                "organizations:ListAWSServiceAccessForOrganization",
                "organizations:ListCreateAccountStatus",
                "organizations:ListOrganizationalUnitsForParent",
                "organizations:ListParents",
                "organizations:ListPolicies",
                "organizations:ListPoliciesForTarget",
                "organizations:ListRoots",
                "organizations:ListTargetsForPolicy",
                "organizations:UpdatePolicy",
            ],
            "Resource": "*"
        }
    ]
}
```

For more information about IAM policies and permissions, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.