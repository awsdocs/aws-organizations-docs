# Inviting an AWS account to join your organization<a name="orgs_manage_accounts_invites"></a>

After you create an organization and verify that you own the email address associated with the management account, you can invite existing AWS accounts to join your organization\.

When you invite an account, AWS Organizations sends an invitation to the account owner, who decides whether to accept or decline the invitation\. You can use the AWS Organizations console to initiate and manage invitations that you send to other accounts\. You can send an invitation to another account only from the management account of your organization\.

If you are the administrator of an AWS account, you also can accept or decline an invitation from an organization\. If you accept, your account becomes a member of that organization\. Your account can join only one organization, so if you receive multiple invitations to join, you can accept only one\.

At the moment an account accepts the invitation to join an organization, the management account of the organization becomes liable for all charges accrued by the new member account\. The payment method attached to the member account is no longer used\. Instead, the payment method attached to the management account of the organization pays for all charges accrued by the member account\.

When an invited account joins your organization, you *do not* automatically have full administrator control over the account, unlike created accounts\. If you want the management account to have full administrative control over an invited member account, you must create the `OrganizationAccountAccessRole` IAM role in the member account and grant permission to the management account to assume the role\. To configure this, after the invited account becomes a member, follow the steps in [Creating the OrganizationAccountAccessRole in an invited member account](orgs_manage_accounts_access.md#orgs_manage_accounts_create-cross-account-role)\.

**Note**  
When you create an account in your organization instead of inviting an existing account to join, AWS Organizations automatically creates an IAM role \(named `OrganizationAccountAccessRole` by default\) that you can use to grant users in the management account administrator access to the created account\. 

AWS Organizations *does* automatically create a service\-linked role in invited member accounts to support integration between AWS Organizations and other AWS services\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

For the number of invitations you can send per day, see [Maximum and minimum values](orgs_reference_limits.md#min-max-values)\. Accepted invitations don't count against this quota\. As soon as one invitation is accepted, you can send another invitation that same day\. Each invitation must be responded to within 15 days, or it expires\.

An invitation that is sent to an account counts against the quota of accounts in your organization\. The count is restored if the invited account declines, the management account cancels the invitation, or the invitation expires\.

To create an account that automatically is part of your organization, see [Creating an AWS account in your organization](orgs_manage_accounts_create.md)\.

**Important**  
Because of legal and billing constraints, you can invite AWS accounts only from the same AWS seller and AWS partition as the management account\.  
All accounts in an organization must come from the same seller of record as the management account\. If one account comes from AWS in the United States, and another account comes from an AWS reseller in the European Union, and another from an AWS reseller in India, then they can't be in the same organization\.
All accounts in an organization must come from the same AWS partition as the management account\. Accounts in the commercial AWS Regions partition can't be in an organization with accounts from the China Regions partition or accounts in the AWS GovCloud \(US\) Regions partition\.

## Sending invitations to AWS accounts<a name="orgs_manage_accounts_invite-account"></a>

To invite accounts to your organization, you must first verify that you own the email address associated with the management account\. For more information, see [Email address verification](orgs_manage_org_create.md#about-email-verification)\. After you verify your email address, complete the following steps to invite accounts to your organization\.

**Minimum permissions**  
To invite an AWS account to join your organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:InviteAccountToOrganization`

------
#### [ AWS Management Console ]

**To invite another account to join your organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. If you already verified your email address with AWS, skip this step\.

   If you haven't yet verified your email address, follow the instructions in the [verification email](orgs_manage_org_create.md#about-email-verification) within 24 hours after you create the organization\. There might be a delay before you receive the verification email message\. You can't invite an account to join your organization until you verify your email address\. 

1. Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, and choose **Add an AWS account**\.

1. On the**[Add an AWS account](https://console.aws.amazon.com/organizations/v2/home/accounts/add/create)** page, choose **Invite an existing AWS account**\.

1. On the **[Invite an existing AWS](https://console.aws.amazon.com/organizations/v2/home/accounts/add/invite)** page, for **Email address or account ID of the AWS account to invite** enter either the email address associated with the account to be invited, or its account ID number\. 

1. \(Optional\) For **Message to include in the invitation email message**, enter any text that you want to include in the email invitation to the invited account owner\.

1. \(Optional\) In the **Add tags** section, specify one or more tags that are automatically applied to the account after its administrator accepts the invitation\. To do this, choose **Add tag** and then enter a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to an AWS account\.

1. Choose **Send invitation**\.
**Important**  
If you get a message that you exceeded your account quotas for the organization or that you can't add an account because your organization is still initializing, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

1. The console redirects you to the **[Invitations](https://console.aws.amazon.com/organizations/v2/home/accounts/invitations)** page page where you can view all open and accepted invitations here\. The invitation that you just created appears at the top of the list with its status set to **OPEN**\.

   AWS Organizations sends an invitation to the email address of the owner of the account that you invited to the organization\. This email message includes a link to the AWS Organizations console, where the account owner can view the details and choose to accept or decline the invitation\. Alternatively, the owner of the invited account can bypass the email message, go directly to the AWS Organizations console, view the invitation, and accept or decline it\.

   The invitation to this account immediately counts against the maximum number of accounts that you can have in your organization\. AWS Organizations doesn't wait until the account accepts the invitation\. If the invited account declines, the management account cancels the invitation\. If the invited account doesn't respond within the specified time period, the invitation expires\. In either case, the invitation no longer counts against your quota\.

------
#### [ AWS CLI & AWS SDKs ]

**To invite another account to join your organization**  
You can use one of the following commands to invite another account to join your organization:
+ AWS CLI: [invite\-account\-to\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/invite-account-to-organization.html) 

  ```
  $ aws organizations invite-account-to-organization \
      --target '{"Type": "EMAIL", "Id": "juan@example.com"}' \
      --notes "This is a request for Juan's account to join Bill's organization."
  {
      "Handshake": {
          "Action": "INVITE",
          "Arn": "arn:aws:organizations::111111111111:handshake/o-exampleorgid/invite/h-examplehandshakeid111",
          "ExpirationTimestamp": 1482952459.257,
          "Id": "h-examplehandshakeid111",
          "Parties": [
              {
                  "Id": "o-exampleorgid",
                   "Type": "ORGANIZATION"
              },
              {
                   "Id": "juan@example.com",
                   "Type": "EMAIL"
              }
          ],
          "RequestedTimestamp": 1481656459.257,
          "Resources": [
              {
                  "Resources": [
                      {
                          "Type": "MASTER_EMAIL",
                          "Value": "bill@amazon.com"
                      },
                      {
                           "Type": "MASTER_NAME",
                           "Value": "Management Account"
                      },
                      {
                           "Type": "ORGANIZATION_FEATURE_SET",
                           "Value": "FULL"
                      }
                  ],
                  "Type": "ORGANIZATION",
                  "Value": "o-exampleorgid"
              },
              {
                  "Type": "EMAIL",
                  "Value": "juan@example.com"
              }
          ],
          "State": "OPEN"
      }
  }
  ```
+ AWS SDKs: [InviteAccountToOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_InviteAccountToOrganization.html)

------

## Managing pending invitations for your organization<a name="orgs_manage_accounts_manage-invites"></a>

When you sign in to your management account, you can view all the linked AWS accounts in your organization and cancel any pending \(open\) invitations\. To do this, complete the following steps\.

**Minimum permissions**  
To manage pending invitations for your organization, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:ListHandshakesForOrganization` 
`organizations:CancelHandshake`

------
#### [ AWS Management Console ]

**To view or cancel invitations that are sent from your organization to other accounts**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Invitations](https://console.aws.amazon.com/organizations/v2/home/accounts/invitations)** page\. 

   This page displays all invitations that are sent from your organization and their current status\.
**Note**  
Accepted, canceled, and declined invitations continue to appear in the list for 30 days\. After that, they're deleted and no longer appear in the list\.

1. Choose the radio button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/radio-button-selected.png)next to the invitation that you want to cancel, and then choose **Cancel invitation**\. If the radio button is grayed out, then that invitation can't be canceled\.

   The status of the invitation changes from **OPEN** to **CANCELED**\.

   AWS sends an email message to the account owner stating that you canceled the invitation\. The account can no longer join the organization unless you send a new invitation\.

------
#### [ AWS CLI & AWS SDKs ]

**To view or cancel invitations that are sent from your organization to other accounts**  
You can use the following commands to view or cancel invitations:
+ AWS CLI: [list\-handshakes\-for\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-handshakes-for-organization.html), [cancel\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/cancel-handshake.html) 
+ The following example shows the invitations sent by this organization to other accounts\.

  ```
  $ aws organizations list-handshakes-for-organization
  {
      "Handshakes": [
          {
              "Action": "INVITE",
              "Arn": "arn:aws:organizations::111111111111:handshake/o-exampleorgid/invite/h-examplehandshakeid111",
              "ExpirationTimestamp": 1482952459.257,
              "Id": "h-examplehandshakeid111",
              "Parties": [
                  {
                      "Id": "o-exampleorgid",
                      "Type": "ORGANIZATION"
                  },
                  {
                      "Id": "juan@example.com",
                      "Type": "EMAIL"
                  }
              ],
              "RequestedTimestamp": 1481656459.257,
              "Resources": [
                  {
                      "Resources": [
                          {
                              "Type": "MASTER_EMAIL",
                              "Value": "bill@amazon.com"
                          },
                          {
                              "Type": "MASTER_NAME",
                              "Value": "Management Account"
                          },
                          {
                              "Type": "ORGANIZATION_FEATURE_SET",
                              "Value": "FULL"
                          }
                      ],
                      "Type": "ORGANIZATION",
                      "Value": "o-exampleorgid"
                  },
                  {
                      "Type": "EMAIL",
                      "Value": "juan@example.com"
                  },
                  {
                      "Type":"NOTES",
                      "Value":"This is an invitation to Juan's account to join Bill's organization."
                  }
              ],
              "State": "OPEN"
          },
          {
              "Action": "INVITE",
              "State":"ACCEPTED",
              "Arn": "arn:aws:organizations::111111111111:handshake/o-exampleorgid/invite/h-examplehandshakeid111",
              "ExpirationTimestamp": 1.471797437427E9,
              "Id": "h-examplehandshakeid222",
              "Parties": [
                  {
                      "Id": "o-exampleorgid",
                      "Type": "ORGANIZATION"
                  },
                  {
                      "Id": "anika@example.com",
                      "Type": "EMAIL"
                  }
              ],
              "RequestedTimestamp": 1.469205437427E9,
              "Resources": [
                  {
                      "Resources": [
                          {
                              "Type":"MASTER_EMAIL",
                               "Value":"bill@example.com"
                          },
                          {
                              "Type":"MASTER_NAME",
                              "Value":"Management Account"
                          }
                      ],
                      "Type":"ORGANIZATION",
                      "Value":"o-exampleorgid"
                  },
                  {
                      "Type":"EMAIL",
                      "Value":"anika@example.com"
                  },
                  {
                      "Type":"NOTES",
                      "Value":"This is an invitation to Anika's account to join Bill's organization."
                  }
              ]
          }
      ]
  }
  ```

  The following example shows how to cancel an invitation to an account\.

  ```
  $ aws organizations cancel-handshake --handshake-id h-examplehandshakeid111
  {
      "Handshake": {
          "Id": "h-examplehandshakeid111",
          "State":"CANCELED",
          "Action": "INVITE",
          "Arn": "arn:aws:organizations::111111111111:handshake/o-exampleorgid/invite/h-examplehandshakeid111",
          "Parties": [
              {
                  "Id": "o-exampleorgid",
                  "Type": "ORGANIZATION"
              },
              {
                  "Id": "susan@example.com",
                  "Type": "EMAIL"
              }
          ],
          "Resources": [
              {
                  "Type": "ORGANIZATION",
                  "Value": "o-exampleorgid",
                  "Resources": [
                      {
                          "Type": "MASTER_EMAIL",
                          "Value": "bill@example.com"
                      },
                      {
                          "Type": "MASTER_NAME",
                          "Value": "Management Account"
                      },
                      {
                          "Type": "ORGANIZATION_FEATURE_SET",
                          "Value": "CONSOLIDATED_BILLING"
                      }
                  ]
              },
              {
                  "Type": "EMAIL",
                  "Value": "anika@example.com"
              },
              {
                  "Type": "NOTES",
                  "Value": "This is a request for Susan's account to join Bob's organization."
              }
          ],
          "RequestedTimestamp": 1.47008383521E9,
          "ExpirationTimestamp": 1.47137983521E9
      }
  }
  ```
+ AWS SDKs: [ListHandshakesForOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListHandshakesForOrganization.html), [CancelHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CancelHandshake.html)

------

## Accepting or declining an invitation from an organization<a name="orgs_manage_accounts_accept-decline-invite"></a>

Your AWS account might receive an invitation to join an organization\. You can accept or decline the invitation\. To do this, complete the following steps\.

**Note**  
An account’s status with an organization affects what cost and usage data is visible:  
If a member account leaves an organization and becomes a standalone account, the account no longer has access to cost and usage data from the time range when the account was a member of the organization\. The account has access only to the data that is generated as a standalone account\. 
If a member account leaves organization A to join organization B, the account no longer has access to cost and usage data from the time range when the account was a member of organization A\. The account has access only to the data that is generated as a member of organization B\. 
If an account rejoins an organization that it previously belonged to, the account regains access to its historical cost and usage data\.

**Minimum permissions**  
To accept or decline an invitation to join an AWS organization, you must have the following permissions:  
`organizations:ListHandshakesForAccount` – Required to see the list of invitations in the AWS Organizations console\.
`organizations:AcceptHandshake`\.
`organizations:DeclineHandshake`\.
`iam:CreateServiceLinkedRole` – Required only when accepting the invitation requires the creation of a service\-linked role in the member account to support integration with other AWS services\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

------
#### [ AWS Management Console ]

**To accept or decline an invitation**

1. An invitation to join an organization is sent to the email address of the account owner\. If you are an account owner and you receive an invitation email message, follow the instructions in the email invitation or go to [AWS Organizations console](https://console.aws.amazon.com/organizations/v2) in your browser, and then choose **Invitations**, or go straight to the **[member account's Invitation](https://console.aws.amazon.com/organizations/v2/home/invitations)** page\.

1. If prompted, sign in to the invited account as an IAM user, assume an IAM role, or sign in as the account's root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. The **[member account's Invitation](https://console.aws.amazon.com/organizations/v2/home/invitations)** page displays your account's open invitations to join organizations\.

   Choose **Accept invitation** or **Decline invitation** as appropriate\.
   + If you choose **Accept invitation** in the preceding step, the console redirects you to the [Organization overview](https://console.aws.amazon.com/organizations/v2/home/dashboard) page with details about the organization that your account is now a member of\. You can view the organization's ID and the owner's email address\.
**Note**  
Accepted invitations continue to appear in the list for 30 days\. After that, they are deleted and no longer appear in the list\.

     AWS Organizations automatically creates a service\-linked role in the new member account to support integration between AWS Organizations and other AWS services\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

     AWS sends an email message to the owner of the organization's management account stating that you accepted the invitation\. It also sends an email message to the member account owner stating that the account is now a member of the organization\.
   + If you choose **Decline** in the preceding step, your account remains on the **[member account's Invitation](https://console.aws.amazon.com/organizations/v2/home/invitations)** page that lists any other pending invitations\.

     AWS sends an email message to the organization's management account owner stating that you declined the invitation\.
**Note**  
Declined invitations continue to appear in the list for 30 days\. After that, they are deleted and no longer appear in the list\.

------
#### [ AWS CLI & AWS SDKs ]

**To accept or decline an invitation**  
You can use the following commands to accept or decline an invitation:
+ AWS CLI: [accept\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/accept-handshake.html), [decline\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/decline-handshake.html) 

  The following example shows how to accept an invitation to join an organization\.

  ```
  $ aws organizations accept-handshake --handshake-id h-examplehandshakeid111
  {
      "Handshake": {
          "Action": "INVITE",
          "Arn": "arn:aws:organizations::111111111111:handshake/o-exampleorgid/invite/h-examplehandshakeid111",
          "RequestedTimestamp": 1481656459.257,
          "ExpirationTimestamp": 1482952459.257,
          "Id": "h-examplehandshakeid111",
          "Parties": [
              {
                  "Id": "o-exampleorgid",
                  "Type": "ORGANIZATION"
              },
              {
                  "Id": "juan@example.com",
                  "Type": "EMAIL"
              }
          ],
          "Resources": [
              {
                  "Resources": [
                      {
                          "Type": "MASTER_EMAIL",
                          "Value": "bill@amazon.com"
                      },
                      {
                          "Type": "MASTER_NAME",
                          "Value": "Management Account"
                      },
                      {
                          "Type": "ORGANIZATION_FEATURE_SET",
                           "Value": "ALL"
                      }
                  ],
                  "Type": "ORGANIZATION",
                  "Value": "o-exampleorgid"
              },
              {
                  "Type": "EMAIL",
                  "Value": "juan@example.com"
              }
          ],
          "State": "ACCEPTED"
      }
  }
  ```

  The following example shows how to decline an invitation to join an organization\.
+ AWS SDKs: [AcceptHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AcceptHandshake.html), [DeclineHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeclineHandshake.html)

------