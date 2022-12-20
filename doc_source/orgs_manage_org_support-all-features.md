# Enabling all features in your organization<a name="orgs_manage_org_support-all-features"></a>

AWS Organizations has two available feature sets:
+ [All features](orgs_getting-started_concepts.md#feature-set-all) – This feature set is the preferred way to work with AWS Organizations, and it includes Consolidating Billing features\. When you create an organization, enabling all features is the default\. With all features enabled, you can use the advanced account management features available in AWS Organizations such as [integration with supported AWS services](orgs_integrate_services_list.md) and [organization management policies](orgs_manage_policies.md)\.
+ [Consolidated Billing features](orgs_getting-started_concepts.md#feature-set-cb-only) – All organizations support this subset of features, which provides basic management tools that you can use to centrally manage the accounts in your organization\. 

If you create an organization with consolidated billing features only, you can later enable all features\. This page describes the process of enabling all features\.

## Before enabling all features<a name="before-enabling-all"></a>

Before changing from an organization that supports only consolidated billing features to an organization supporting all features, note the following:
+ When you start the process to enable all features, AWS Organizations sends a request to every member account that you *invited* to join your organization\. Every invited account must approve enabling all features by accepting the request\. Only then can you complete the process to enable all features in your organization\. If an account declines the request, you must either remove the account from your organization or resend the request\. The request must be accepted before you can complete the process to enable all features\. Accounts that you *created* using AWS Organizations don't get a request because they don't need to approve the additional control\. 
+ You can continue inviting accounts to your organization while enabling all features\. The owner of an invited account is informed by the invitation whether they are joining an organization with consolidated billing only, or with all features enabled\.
  + If you invite an account *during* the process to enable all features, the invitation states that the organization they are joining has all features enabled\. If you cancel the process to enable all features before the account accepts the invitation, that invitation is canceled\. You must invite the account again to be a member of an organization with consolidated billing features only\.
  + If you invite an account and the invitation is not yet accepted *before* you begin the process to enable all features, that invitation is canceled because the invitation states that the organization has consolidated billing features only\. You must invite the account again to be a member of an organization with all features enabled\. 
+ You can also continue creating accounts in the organization\. That process isn't affected by this change\.
+ AWS Organizations verifies that every member account has a service\-linked role named `AWSServiceRoleForOrganizations`\. This role is mandatory in all accounts to enable all features\. If you deleted the role in an invited account, accepting the invitation to enable all features recreates the role\. If you deleted the role in an account that was created using AWS Organizations, that account receives an invitation specifically to recreate that role\. All of these invitations must be accepted for the organization to complete the process of enabling all features\.
+ Because enabling all features makes it possible to use [SCPs](orgs_manage_policies_scps.md), be sure that your account administrators understand the effects of attaching SCPs to the organization, organizational units, or accounts\. SCPs can restrict what users and even administrators can do in affected accounts\. For example, the management account can apply SCPs that can prevent member accounts from leaving the organization\.
+ The management account isn't affected by any SCP\. You can't limit what users and roles in the management account can do by applying SCPs\. SCPs affect only member accounts\.
+ The migration from consolidated billing features to all features is one\-way\. You can't switch an organization with all features enabled back to consolidated billing features only\.
+ \(Not recommended\) If your organization has only consolidated billing features enabled, member account administrators can choose to delete the service\-linked role named `AWSServiceRoleForOrganizations`\. If you later choose to enable all features in an organization, this role is required and is recreated in all accounts as part of accepting the invitation to enable all features\. For more information about how AWS Organizations uses this role, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

## Beginning the process to enable all features<a name="manage-begin-all-features"></a>

When you sign in to your organization's management account, you can begin the process to enable all features\. To do this, complete the following steps\.

**Minimum permissions**  
To enable all features in your organization, you must have the following permission:  
`organizations:EnableAllFeatures`
`organizations:DescribeOrganization` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To ask your invited member accounts to agree to enable all features in the organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Settings](https://console.aws.amazon.com/organizations/v2/home/settings)** page choose **Begin process**\.

1. On the **[Enable all features](https://console.aws.amazon.com/organizations/v2/home/settings/enable-all-features)** page, acknowledge your understanding that you cannot return to only consolidated billing features after you switch by choosing **Begin process**\. 

   AWS Organizations sends a request to every invited \(not created\) account in the organization asking for approval to enable all features in the organization\. If you have any accounts that were created using AWS Organizations and the member account administrator deleted the service\-linked role named `AWSServiceRoleForOrganizations`, AWS Organizations sends that account a request to recreate the role\.

   The console displays the **Request approval status** list for the invited accounts\.
**Tip**  
To get back to this page later, open the **[Settings](https://console.aws.amazon.com/organizations/v2/home/settings)** page and in the **Request sent *date*** section, choose **View status**\.

1. The **[Enable all features](https://console.aws.amazon.com/organizations/v2/home/settings/enable-all-features)** page shows the current request status for each account in the organization\. Accounts that have agreed to the request show a status of **ACCEPTED**\. Accounts that haven't yet agreed show a status of **OPEN**\.

------
#### [ AWS CLI & AWS SDKs ]

**To ask your invited member accounts to agree to enable all features in the organization**  
You can use one of the following commands to enable all features in an organization: 
+ AWS CLI: [enable\-all\-features](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-all-features.html)

  The following command begins the process to enable all features in the organization\.

  ```
  $ aws organizations enable-all-features
  {
      "Handshake": {
          "Id": "h-79d8f6f114ee4304a5e55397eEXAMPLE",
          "Arn": "arn:aws:organizations::123456789012:handshake/o-aa111bb222/enable_all_features/h-79d8f6f114ee4304a5e55397eEXAMPLE",
          "Parties": [
              {
                  "Id": "a1b2c3d4e5",
                  "Type": "ORGANIZATION"
              }
          ],
          "State": "REQUESTED",
          "RequestedTimestamp": "2020-11-19T16:21:46.995000-08:00",
          "ExpirationTimestamp": "2021-02-17T16:21:46.995000-08:00",
          "Action": "ENABLE_ALL_FEATURES",
          "Resources": [
              {
                  "Value": "o-a1b2c3d4e5",
                  "Type": "ORGANIZATION"
              }
          ]
      }
  }
  ```

  The output shows the details of the handshake that invited member accounts must agree to\.
+ AWS SDKs: [EnableAllFeatures](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAllFeatures.html)

------

**Notes**  
A countdown of 90 days begins when the request is sent to the member accounts\. All accounts must approve the request within that time period or the request expires\. If the request expires, all requests related to this attempt are canceled, and you have to start over with step 2\.
During the time between when you make the request to enable all features and when either all accounts accept or the timeout occurs, all pending invitations for other accounts to join the organization are automatically canceled\. You can't issue new invitations until the process of enabling all features is finished\. 
After you complete the process of enabling all features, you once again can invite accounts to join the organization\. The process doesn't change, but all invitations include information letting the recipients know that if they accept the invitation, they're subject to any applicable policies\.

After all invited accounts in the organization approve their requests, you can finalize the process and enable all features\. You can also immediately finalize the process if your organization doesn't have any *invited* member accounts\. To finalizing the process, continue with [Finalizing the process to enable all features](#finalize-migration)\. 

## Approving the request to enable all features or to recreate the service\-linked role<a name="manage-approve-all-features-invite"></a>

When you sign in to one of the organization's invited member accounts, you can approve a request from the management account\. If your account was originally invited to join the organization, the invitation is to enable all features and implicitly includes approval for recreating the `AWSServiceRoleForOrganizations` role, if needed\. If your account was instead created using AWS Organizations and you deleted the `AWSServiceRoleForOrganizations` service\-linked role, you receive an invitation only to recreate the role\. To do this, complete the following steps\.

**Important**  
If you perform the steps in the following procedure, the management account in the organization can apply policy\-based controls on your member account\. These controls can restrict what users and even what you as the administrator can do in your account\. Such restrictions might prevent your account from leaving the organization\.

**Minimum permissions**  
To approve a request to enable all features for your member account, you must have the following permissions:  
`organizations:AcceptHandshake`
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:ListHandshakesForAccount`– required only when using the Organizations console
`iam:CreateServiceLinkedRole` – required only if the `AWSServiceRoleForOrganizations` role must be recreated in the member account

------
#### [ AWS Management Console ]

**To agree to the request to enable all features in the organization**

1. Sign in to the AWS Organizations console at [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in a member account\.

1. Read what accepting the request for all features in the organization means for your account, and then choose **Accept**\. The page continues to show the process as incomplete until all accounts in the organization accept the requests and the administrator of the management account finalizes the process\.

------
#### [ AWS CLI & AWS SDKs ]

**To agree to the request to enable all features in the organization**  
To agree to the request, you must accept the handshake with `"Action": "APPROVE_ALL_FEATURES"`\.
+ AWS CLI:
  +  [accept\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/accept-handshake.html)
  + [list\-handshakes\-for\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-handshakes-for-account.html)

  The following example shows how to list the handshakes available for your account\. The value of `"Id"` in the fourth line of the output is the value you need for the next command\.

  ```
  $ aws organizations list-handshakes-for-account
  {
      "Handshakes": [
          {
              "Id": "h-a2d6ecb7dbdc4540bc788200aEXAMPLE",
              "Arn": "arn:aws:organizations::123456789012:handshake/o-aa111bb222/approve_all_features/h-a2d6ecb7dbdc4540bc788200aEXAMPLE",
              "Parties": [
                  {
                      "Id": "a1b2c3d4e5",
                      "Type": "ORGANIZATION"
                  },
                  {
                      "Id": "111122223333",
                      "Type": "ACCOUNT"
                  }
              ],
              "State": "OPEN",
              "RequestedTimestamp": "2020-11-19T16:35:24.824000-08:00",
              "ExpirationTimestamp": "2021-02-17T16:35:24.035000-08:00",
              "Action": "APPROVE_ALL_FEATURES",
              "Resources": [
                  {
                      "Value": "c440da758cab44068cdafc812EXAMPLE",
                      "Type": "PARENT_HANDSHAKE"
                  },
                  {
                      "Value": "o-aa111bb222",
                      "Type": "ORGANIZATION"
                  },
                  {
                      "Value": "111122223333",
                      "Type": "ACCOUNT"
                  }
              ]
          }
      ]
  }
  ```

  The following example uses the Id of the handshake from the previous command to accept that handshake\.

  ```
  $ aws organizations accept-handshake --handshake-id h-a2d6ecb7dbdc4540bc788200aEXAMPLE
  {
      "Handshake": {
          "Id": "h-a2d6ecb7dbdc4540bc788200aEXAMPLE",
          "Arn": "arn:aws:organizations::123456789012:handshake/o-aa111bb222/approve_all_features/h-a2d6ecb7dbdc4540bc788200aEXAMPLE",
          "Parties": [
              {
                  "Id": "a1b2c3d4e5",
                  "Type": "ORGANIZATION"
              },
              {
                  "Id": "111122223333",
                  "Type": "ACCOUNT"
              }
          ],
          "State": "ACCEPTED",
          "RequestedTimestamp": "2020-11-19T16:35:24.824000-08:00",
          "ExpirationTimestamp": "2021-02-17T16:35:24.035000-08:00",
          "Action": "APPROVE_ALL_FEATURES",
          "Resources": [
              {
                  "Value": "c440da758cab44068cdafc812EXAMPLE",
                  "Type": "PARENT_HANDSHAKE"
              },
              {
                  "Value": "o-aa111bb222",
                  "Type": "ORGANIZATION"
              },
              {
                  "Value": "111122223333",
                  "Type": "ACCOUNT"
              }
          ]
      }
  }
  ```
+ AWS SDKs:
  + [list\-handshakes\-for\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-handshakes-for-account.html)
  + [AcceptHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AcceptHandshake.html)

------

## Finalizing the process to enable all features<a name="finalize-migration"></a>

All invited member accounts must approve the request to enable all features\. If there are no invited member accounts in the organization, the **Enable all features progress** page indicates with a green banner that you can finalize the process\.

**Minimum permissions**  
To finalize the process to enable all features for the organization, you must have the following permission:  
`organizations:AcceptHandshake`
`organizations:ListHandshakesForOrganization`
`organizations:DescribeOrganization` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To finalize the process to enable all features**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Settings](https://console.aws.amazon.com/organizations/v2/home/settings)** page, if all invited accounts accept the request to enable all features, a green box appears at the top of the page to inform you\. In the green box, choose **Go to finalize**\.

1. On the **[Enable all features](https://console.aws.amazon.com/organizations/v2/home/settings/enable-all-features)** page, choose **Finalize**, and then in the confirmation dialog box, choose **Finalize** again\.

1. The organization now has all features enabled\.

------
#### [ AWS CLI & AWS SDKs ]

**To finalize the process to enable all features**  
To finalize the process, you must accept the handshake with `"Action": "ENABLE_ALL_FEATURES"`\.
+ AWS CLI:
  + [list\-handshakes\-for\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-handshakes-for-organization.html)
  +  [accept\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/accept-handshake.html)

  ```
  $ aws organizations list-handshakes-for-organization
  {
      "Handshakes": [
          {
              "Id": "h-43a871103e4c4ee399868fbf2EXAMPLE",
              "Arn": "arn:aws:organizations::123456789012:handshake/o-aa111bb222/enable_all_features/h-43a871103e4c4ee399868fbf2EXAMPLE",
              "Parties": [
                  {
                      "Id": "a1b2c3d4e5",
                      "Type": "ORGANIZATION"
                  }
              ],
              "State": "OPEN",
              "RequestedTimestamp": "2020-11-20T08:41:48.047000-08:00",
              "ExpirationTimestamp": "2021-02-18T08:41:48.047000-08:00",
              "Action": "ENABLE_ALL_FEATURES",
              "Resources": [
                  {
                      "Value": "o-aa111bb222",
                      "Type": "ORGANIZATION"
                  }
              ]
          }
      ]
  }
  ```

  The following example shows how to list the handshakes available for the organization\. The value of `"Id"` in the fourth line of the output is the value you need for the next command\.

  ```
  $ aws organizations accept-handshake \
      --handshake-id h-43a871103e4c4ee399868fbf2EXAMPLE
  {
      "Handshake": {
          "Id": "h-43a871103e4c4ee399868fbf2EXAMPLE",
          "Arn": "arn:aws:organizations::123456789012:handshake/o-aa111bb222/enable_all_features/h-43a871103e4c4ee399868fbf2EXAMPLE",
          "Parties": [
              {
                  "Id": "a1b2c3d4e5",
                  "Type": "ORGANIZATION"
              }
          ],
          "State": "ACCEPTED",
          "RequestedTimestamp": "2020-11-20T08:41:48.047000-08:00",
          "ExpirationTimestamp": "2021-02-18T08:41:48.047000-08:00",
          "Action": "ENABLE_ALL_FEATURES",
          "Resources": [
              {
                  "Value": "o-aa111bb222",
                  "Type": "ORGANIZATION"
              }
          ]
      }
  }
  ```
+ AWS SDKs:
  + [AcceptHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListHandshakesForOrganization.html)
  + [AcceptHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AcceptHandshake.html)

------

The next steps:
+ Enable the policy types that you want to use\. After that, you can attach policies to administer the accounts in your organization\. For more information, see [Managing AWS Organizations policies](orgs_manage_policies.md)\.
+ Enable integration with supported services\. For more information, see [Using AWS Organizations with other AWS services](orgs_integrate_services.md)\.