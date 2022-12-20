# Example: Consolidated permissions to manage an organization's backup policies<a name="orgs_delegate_policies_example_consolidate_permissions"></a>

This example shows how you might create a resource\-based delegation policy that allows the management account to delegate full permissions necessary to manage backup policies within the organization, including `create`, `read`, `update`, and `delete` actions, as well as `attach` and `detach` policy actions\. To understand the significance of each action, resource and condition, see [Example resource\-based delegation policies](orgs_manage_policies_delegate_examples.md)\. 

**Important**  
This policy allows delegated administrators to perform the specified actions on policies created by any account in the organization, including the management account\.

This example delgation policy grants the permissions necessary to complete actions programmatically from the AWS API or AWS CLI\. To use this delegation policy, replace the AWS placeholder text for *MemberAccountId*, *ManagementAccountId*, *OrganizationId*, and *RootId* with your own information\. Then, follow the directions in [Delegated administrator for AWS Organizations](orgs_delegate_policies.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DelegatingNecessaryDescribeListActions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::MemberAccountId:root"
      },
      "Action": [
        "organizations:DescribeOrganization",
        "organizations:DescribeOrganizationalUnit",
        "organizations:DescribeAccount",
        "organizations:DescribePolicy",
        "organizations:DescribeEffectivePolicy",
        "organizations:ListRoots",
        "organizations:ListOrganizationalUnitsForParent",
        "organizations:ListParents",
        "organizations:ListChildren",
        "organizations:ListAccounts",
        "organizations:ListAccountsForParent",
        "organizations:ListPolicies",
        "organizations:ListPoliciesForTarget",
        "organizations:ListTargetsForPolicy",
        "organizations:ListTagsForResource"
      ],
      "Resource": "*",
      "Condition": {
        "StringLikeIfExists": {
          "organizations:PolicyType": "BACKUP_POLICY"
        }
      }
    },
    {
      "Sid": "DelegatingAllActionsForBackupPolicies",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::MemberAccountId:root"
      },
      "Action": [
        "organizations:CreatePolicy",
        "organizations:UpdatePolicy",
        "organizations:DeletePolicy",
        "organizations:AttachPolicy",
        "organizations:DetachPolicy",
        "organizations:EnablePolicyType",
        "organizations:DisablePolicyType"
      ],
      "Resource": [
        "arn:aws:organizations::ManagementAccountId:root/o-OrganizationId/r-RootId",
        "arn:aws:organizations::ManagementAccountId:ou/o-OrganizationId/*",
        "arn:aws:organizations::ManagementAccountId:account/o-OrganizationId/*",
        "arn:aws:organizations::ManagementAccountId:policy/o-OrganizationId/backup_policy/*"
      ]
    }
  ]
}
```