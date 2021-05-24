# Viewing details about your organization<a name="orgs_manage_org_details"></a>

You can perform the following tasks to view details about elements of your organization\.

**Topics**
+ [Viewing the details of an organization from the management account](#orgs_view_org)
+ [Viewing the details of the root](#orgs_view_root)
+ [Viewing the details of an OU](#orgs_view_ou)
+ [Viewing details of an account](#orgs_view_account)
+ [Viewing details of a policy](#orgs_view_policy)

## Viewing the details of an organization from the management account<a name="orgs_view_org"></a>

When you sign in to the organization's management account in the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2), you can view details of the organization\.

**Minimum permissions**  
To view the details of an organization, you must have the following permission:  
`organizations:DescribeOrganization`

------
#### [ AWS Management Console ]

**To view the details for your organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Settings](https://console.aws.amazon.com/organizations/v2/home/settings)** page\. This page displays details about the organization, including the organization ID and the account name and email address assigned to the organization's management account\.

------
#### [ AWS CLI & AWS SDKs ]

**To view the details for your organization**  
You can use one of the following commands to view details of an organization:
+ AWS CLI: [describe\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-organization.html) 

  The following example shows the information included in the output of this command\.

  ```
  $ aws organizations describe-organization
  {
      "Organization": {
          "Id": "o-aa111bb222",
          "Arn": "arn:aws:organizations::123456789012:organization/o-aa111bb222",
          "FeatureSet": "ALL",
          "MasterAccountArn": "arn:aws:organizations::128716708097:account/o-aa111bb222/123456789012",
          "MasterAccountId": "123456789012",
          "MasterAccountEmail": "admin@example.com",
          "AvailablePolicyTypes": [ ...DEPRECATED - DO NOT USE... ]
      }
  }
  ```
**Important**  
The `AvailablePolicyTypes` field is deprecated and doesn't contain accurate information about the policies enabled in your organization\. To see the accurate and complete list of policy types that are actually enabled for the organization, use the `ListRoots` command, as described in the AWS CLI portion of the following section\.
+ AWS SDKs: [DescribeOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeOrganization.html)

------

## Viewing the details of the root<a name="orgs_view_root"></a>

When you sign in to the organization's management account in the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2), you can view details of the root\.

**Minimum permissions**  
To view the details of the root, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:ListRoots` 

The root is the topmost container in the hierarchy of organizational units \(OUs\) and generally behaves as an OU\. However, as the container at the very top of the hierarchy, changes to the root affect every other OU and every AWS account in the organization\.

------
#### [ AWS Management Console ]<a name="view_details_root_v2"></a>

**To view the details of the root**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, and choose the **Root** OU \(its name, not the radio button\)\. 

1. The **Root** details page appears and displays the details of the root\.

------
#### [ AWS CLI & AWS SDKs ]

**To view the details of the root**  
You can use one of the following commands to view details of a root: 
+ AWS CLI: [list\-roots](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-roots.html) 

  The following example shows how to retrieve the details of the root, including which policy types are currently enabled in the organization:

  ```
  $ aws organizations list-roots
  {
      "Roots": [
          {
              "Id": "r-a1b2",
              "Arn": "arn:aws:organizations::123456789012:root/o-aa111bb222/r-a1b2",
              "Name": "Root",
              "PolicyTypes": [
                  {
                      "Type": "BACKUP_POLICY",
                      "Status": "ENABLED"
                  }
              ]
          }
      ]
  }
  ```
+ AWS SDKs: [ListRoots](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListRoots.html)

------

## Viewing the details of an OU<a name="orgs_view_ou"></a>

When you sign in to the organization's management account in the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2), you can view details of the OUs in your organization\.

**Minimum permissions**  
To view the details of an organizational unit \(OU\), you must have the following permissions:  
`organizations:DescribeOrganizationalUnit`
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:ListOrganizationsUnitsForParent`– required only when using the Organizations console
`organizations:ListRoots` – required only when using the Organizations console

------
#### [ AWS Management Console ]<a name="view_details_ou_v2"></a>

**To view details of an OU**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, choose the name of the OU \(not its radio button\) that you want to examine\. If the OU that you want is a child of another OU, choose the triangle icon next to its parent OU to expand it and see those in the next level of the hierarchy\. Repeat until you find the OU that you want\.

   The **Organizational unit details** box shows the information about the OU\.

------
#### [ AWS CLI & AWS SDKs ]

**To view details of an OU**  
You can use the following commands to view details of an OU:
+ AWS CLI, AWS SDKs: 
  + [list\-roots](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-roots.html) 
  + [list\-children](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-children.html) 
  + [describe\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-organizational-unit.html) 

  The following example shows how to find the ID of on OU using the AWS CLI\. You find the OU ID by traversing the hierarchy starting with the `list-roots` command and then performing `list-children` on the root and iteratively on each of its children until you find the one you want\.

  ```
  $ aws organizations list-roots
  {
      "Roots": [
          {
              "Id": "r-a1b2",
              "Arn": "arn:aws:organizations::123456789012:root/o-aa111bb222/r-a1b2",
              "Name": "Root",
              "PolicyTypes": []
          }
      ]
  }
  $ aws organizations list-children --parent-id r-a1b2 --child-type ORGANIZATIONAL_UNIT
  {
      "Children": [
          {
              "Id": "ou-a1b2-f6g7h111",
              "Type": "ORGANIZATIONAL_UNIT"
          }
      ]
  }
  ```

  After you have the OU's ID, the following example shows how to retrieve the details about the OU\.

  ```
  $ aws organizations describe-organizational-unit --organizational-unit-id ou-a1b2-f6g7h111
  {
      "OrganizationalUnit": {
          "Id": "ou-a1b2-f6g7h111",
          "Arn": "arn:aws:organizations::123456789012:ou/o-aa111bb222/ou-a1b2-f6g7h111",
          "Name": "Production-Apps"
      }
  }
  ```
+ AWS SDKs:
  +  [ListRoots](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListRoots.html)
  +  [ListChildren](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListChildren.html)
  +  [DescribeOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeOrganizationalUnit.html)

------

## Viewing details of an account<a name="orgs_view_account"></a>

When you sign in to the organization's management account in the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2), you can view details about your accounts\.

**Minimum permissions**  
To view the details of an AWS account, you must have the following permissions:  
`organizations:DescribeAccount`
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:ListAccounts` – required only when using the Organizations console

------
#### [ AWS Management Console ]<a name="view_details_account_v2"></a>

**To view details of an AWS account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page and choose the name of the name of the account \(not the radio button\) that you want to examine\. If the account that you want is a child of an OU, you might have to choose the triangle icon ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)next to an OU to expand it and see its children\. Repeat until you find the account\.

   The **Account details** box shows the information about the account\.

------
#### [ AWS CLI & AWS SDKs ]

**To view details of an AWS account**  
You can use the following commands to view details of an account:
+ AWS CLI:
  +  [list\-accounts](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-accounts.html) – lists the details of *all* accounts in the organization
  +  [describe\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-account.html) – lists the details of only the specified account

  Both commands return the same details for each account included in the response\.

  The following example shows how to retrieve the details about a specified account\.

  ```
  $ aws organizations describe-account --account-id 123456789012
  
  {
      "Account": {
          "Id": "123456789012",
          "Arn": "arn:aws:organizations::123456789012:account/o-aa111bb222/123456789012",
          "Email": "admin@example.com",
          "Name": "Example.com Organization's Management Account",
          "Status": "ACTIVE",
          "JoinedMethod": "INVITED",
          "JoinedTimestamp": "2020-11-20T09:04:20.346000-08:00"
      }
  }
  ```
+ AWS SDKs:
  + [ListAccounts](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListAccounts.html)
  + [DescribeAccount](https://docs.aws.amazon.com/organizations/latest/APIReference/PI_DescribeAccount.html)

------

## Viewing details of a policy<a name="orgs_view_policy"></a>

When you sign in to the organization's management account in the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2), you can view details about your policies\.

**Minimum permissions**  
To view the details of a policy, you must have the following permissions:  
`organizations:DescribePolicy`
`organizations:ListPolicies`

------
#### [ AWS Management Console ]<a name="view_details_policy_v2"></a>

**To view the details of a policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Perform one of the following:
   + Navigate to the **[Policies](https://console.aws.amazon.com/organizations/v2/home/policies)** page, and then choose the policy type for the policy that you want to examine\.
   + Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, then navigate to an OU or account to which the policy is attached\. Finally, choose the **Policies** tab to see the list of attached policies\.

1. Choose the name of the policy \(not the radio button\)\.

   On the **Details** page for the policy, you can view all of the information about the policy, including the JSON policy text, and the list of OUs and accounts that the policy is attached to\.

------
#### [ AWS CLI & AWS SDKs ]

**To view the details of a policy**  
You can use one of the following commands to view details of a policy:
+ AWS CLI:
  +  [list\-policies](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies.html)
  +  [describe\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-policy.html) – lists the details of only the specified policy

  The following example shows how to find the policy ID of the policy that you want to examine\. You must specify a policy type, and the command returns all policies of only that type\.

  ```
  $ aws organizations list-policies --filter BACKUP_POLICY
  {
      "Policies": [
          {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/backup_policy/p-i9j8k7l6m5",
              "Name": "test-backup-policy",
              "Description": "test-policy-description",
              "Type": "BACKUP_POLICY",
              "AwsManaged": false
          }
      ]
  }
  ```

  The response includes all of the details except the JSON policy document\. 

  The following example shows how to retrieve the details of only the specified policy, including the JSON policy document\.

  ```
  $ aws organizations list-policies --policy-id p-i9j8k7l6m5
  {
      "Policies": [
          {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/backup_policy/p-i9j8k7l6m5",
              "Name": "test-backup-policy",
              "Description": "test-policy-description",
              "Type": "BACKUP_POLICY",
              "AwsManaged": false
          },
          "Content": "{\"plans\":{\"My-Backup-Plan\":{\"regions\":{\"@@assign\":[\"us-west-2\"]},\"rules\":{\"My-Backup-Rule\"
                     :{\"target_backup_vault_name\":{\"@@assign\":\"My-Primary-Backup-Vault\"}}},\"selections\":{\"tags\":{
                     \"My-Backup-Plan-Resource-Assignment\":{\"iam_role_arn\":{\"@@assign\":\"arn:aws:iam::$account:role/
                     My-Backup-Role\"},\"tag_key\":{\"@@assign\":\"Stage\"},\"tag_value\":{\"@@assign\":[\"Production\"]}}}}}}}"
      ]
  }
  ```
+ AWS SDKs:
  + [ListPolicies](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPolicies.html)
  + [DescribePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribePolicy.html)

------