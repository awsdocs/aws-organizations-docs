# Example: View organization, OUs, accounts, and policies<a name="orgs_delegate_policies_example_view_accts_orgs"></a>

 Before delegating the management of policies, you must delegate the permissions to navigate the structure of an organization and see the organizational units \(OUs\), accounts, and the policies attached to them\. 

This example shows how you might include these permissions in your resource\-based delegation policy for the member account, *AccountId*\.

**Important**  
It is advisable that you include permissions to only the minimum required actions as shown in the example, although it's possible to delegate any Organizations read\-only action using this policy\.

This example delegation policy grants the permissions necessary to complete actions programmatically from the AWS API or AWS CLI\. To use this delegation policy, replace the AWS placeholder text for *AccountId* with your own information\. Then, follow the directions in [Delegated administrator for AWS Organizations](orgs_delegate_policies.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DelegatingNecessaryDescribeListActions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::AccountId:root"
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
      "Resource": "*"
    }
  ]
}
```