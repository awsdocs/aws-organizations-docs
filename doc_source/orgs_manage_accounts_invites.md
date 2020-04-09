# Inviting an AWS account to join your organization<a name="orgs_manage_accounts_invites"></a>

After you create an organization and verify that you own the email address associated with the master account, you can invite existing AWS accounts to join your organization\. 

When you invite an account, AWS Organizations sends an invitation to the account owner, who decides whether to accept or decline the invitation\. You can use the AWS Organizations console to initiate and manage invitations that you send to other accounts\. You can send an invitation to another account only from the master account of your organization\.

If you are the administrator of an AWS account, you also can accept or decline an invitation from an organization\. If you accept, your account becomes a member of that organization\. Your account can join only one organization, so if you receive multiple invitations to join, you can accept only one\.

When an invited account joins your organization, you *do not* automatically have full administrator control over the account, unlike created accounts\. If you want the master account to have full administrative control over an invited member account, you must create the `OrganizationAccountAccessRole` IAM role in the member account and grant permission to the master account to assume the role\. To configure this, after the invited account becomes a member, follow the steps in [Creating the OrganizationAccountAccessRole in an invited member account](orgs_manage_accounts_access.md#orgs_manage_accounts_create-cross-account-role)\.

**Note**  
When you create an account in your organization instead of inviting an existing account to join, AWS Organizations automatically creates an IAM role \(named `OrganizationAccountAccessRole` by default\) that you can use to grant users in the master account administrator access to the created account\. 

AWS Organizations *does* automatically create a service\-linked role in invited member accounts to support integration between AWS Organizations and other AWS services\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

You can send up to 20 invitations per day per organization\. Accepted invitations don't count against this quota\. As soon as one invitation is accepted, you can send another invitation that same day\. Each invitation must be responded to within 15 days, or it expires\.

An invitation that is sent to an account counts against the quota of accounts in your organization\. The count is restored if the invited account declines, the master account cancels the invitation, or the invitation expires\.

To create an account that automatically is part of your organization, see [Creating an AWS account in your organization](orgs_manage_accounts_create.md)\.

**Important**  
Because of legal and billing constraints, you can invite AWS accounts only from the same AWS seller as the master account\. You can't mix accounts from AWS, Amazon Internet Services Pvt\. Ltd \(AISPL, an AWS seller in India\), or Amazon Connect Technology Services \(Beijing\) Co\. \(ACTS, an AWS seller in China\) in the same organization\. You can add accounts from an AWS seller only to an organization with accounts from the same AWS seller\.

## Sending invitations to AWS accounts<a name="orgs_manage_accounts_invite-account"></a>

To invite accounts to your organization, you must first verify that you own the email address associated with the master account\. After you have verified your email address, complete the following steps to invite accounts to your organization\.

**Minimum permissions**  
To invite an AWS account to join your organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:InviteAccountToOrganization`

**To invite another account to join your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. If your email address is already verified, skip this step\.

   If your email address isn't verified yet, follow the instructions in the [verification email](orgs_manage_create.md#about-email-verification) within 24 hours\. There might be a delay before you receive the verification email\. You can't invite an account until your email address is verified\. 

1. On the **Accounts** tab, choose **Add account**\.

1. Choose **Invite account**\.

1. Enter either the email address or the account ID number of the AWS account that you want to invite to your organization\. If you want to invite multiple accounts, separate them with commas\.

1. \(Optional\) For **Notes**, enter any message that you want included in the email invitation to the other account owners\.

1. Choose **Invite**\.
**Important**  
If you get a message that you exceeded your account quotas for the organization or that you can't add an account because your organization is still initializing, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

1. The console redirects you to the **Invitations** tab\. View all open and accepted invitations here\. The invitation that you just created appears at the top of the list with its status set to **OPEN**\.

   AWS Organizations sends an invitation to the email address of the owner of the account that you invited to the organization\. This email includes a link to the AWS Organizations console, where the account owner can view the details and choose to accept or decline the invitation\. Alternatively, the owner of the invited account can bypass the email, go directly to the AWS Organizations console, view the invitation, and accept or decline it\.

   The invitation to this account immediately counts against the maximum number of accounts that you can have in your organization\. AWS Organizations doesn't wait until the account accepts the invitation\. If the invited account declines, the master account cancels the invitation\. If the invited account doesn't respond within the specified time period, the invitation expires\. In either case, the invitation no longer counts against your quota\.

**To invite another account to join your organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to invite another account to join your organization:
+ AWS CLI: [aws organizations invite\-account\-to\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/invite-account-to-organization.html) 
+ AWS API: [InviteAccountToOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_InviteAccountToOrganization.html)

## Managing pending invitations for your organization<a name="orgs_manage_accounts_manage-invites"></a>

When signed in to your master account, you can view all the linked AWS accounts in your organization and cancel any pending \(open\) invitations\. To do this, complete the following steps\.

**Minimum permissions**  
To manage pending invitations for your organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:ListHandshakesForOrganization`
`organizations:CancelHandshake`

**To view or cancel invitations that are sent from your organization to other accounts \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Choose the **Invitations** tab\. All invitations that are sent from your organization and their current status are listed here\.
**Note**  
Accepted, canceled, and declined invitations continue to appear in the list for 30 days\. After that, they're deleted and no longer appear in the list\.

1. For any open invitations that you want to cancel, under the **Actions** column, choose **Cancel**\.

   The status of the invitation changes from **Open** to **Canceled**\.

   AWS sends an email to the account owner stating that you canceled the invitation\. The account can no longer join the organization unless you send a new invitation\.

**To view or cancel invitations that are sent from your organization to other accounts \(AWS CLI, AWS API\)**  
You can use the following commands to view or cancel invitations:
+ AWS CLI: [aws organizations list\-handshakes\-for\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-handshakes-for-organization.html), [aws organizations cancel\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/cancel-handshake.html) 
+ AWS API: [ListHandshakesForOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListHandshakesForOrganization.html), [CancelHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CancelHandshake.html)

## Accepting or declining an invitation from an organization<a name="orgs_manage_accounts_accept-decline-invite"></a>

Your AWS account might receive an invitation to join an organization\. You can accept or decline the invitation\. To do this, complete the following steps\.

**Minimum permissions**  
To accept or decline an invitation to join an AWS organization, you must have the following permissions:  
`organizations:ListHandshakesForAccount` – Required to see the list of invitations in the AWS Organizations console\.
`organizations:AcceptHandshake`\.
`organizations:DeclineHandshake`\.
`iam:CreateServiceLinkedRole` – Required only when accepting the invitation requires the creation of a service\-linked role to support integration with other AWS services\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

**Note**  
An account’s status with an organization affects what cost and usage data is visible:  
When a standalone account joins an organization, the account no longer has access to cost and usage data from the time range when the account was a standalone account\.
If a member account leaves an organization and becomes a standalone account, the account no longer has access to cost and usage data from the time range when the account was a member of the organization\. The account has access only to the data that is generated as a standalone account\. 
If a member account leaves organization A to join organization B, the account no longer has access to cost and usage data from the time range when the account was a member of organization A\. The account has access only to the data that is generated as a member of organization B\. 
If an account rejoins an organization that it previously belonged to, the account regains access to its historical cost and usage data\.

**To accept or decline an invitation \(console\)**

1. An invitation to join an organization is sent to the email address of the account owner\. If you are an account owner and you receive an invitation email, follow the instructions in the email invitation or go to [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/) in your browser, and then choose **Respond to invitations**\.

1. If prompted, sign in to the invited account as an IAM user, assume an IAM role, or sign in as the account's root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. On the **Invitations** page in the console, you can see your open invitations to join organizations\. Choose **Accept** or **Decline** as appropriate\.
   + If you choose **Accept** in the preceding step, in the **Confirm joining the organization** confirmation window, choose **Confirm**\.

     The console redirects you to the **Organization overview** page with details about the organization that your account is now a member of\. You can view the organization's ID and the owner's email address\.
**Note**  
Accepted invitations continue to appear in the list for 30 days\. After that, they are deleted and no longer appear in the list\.

     AWS Organizations automatically creates a service\-linked role in the new member account to support integration between AWS Organizations and other AWS services\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

     AWS sends an email to the owner of the organization's master account stating that you accepted the invitation\. It also sends an email to the member account owner stating that the account is now a member of the organization\.
   + If you choose **Decline** in the preceding step, your account remains on the **Invitations** page that lists any other pending invitations\.

     AWS sends an email to the organization's master account owner stating that you declined the invitation\.
**Note**  
Declined invitations continue to appear in the list for 30 days\. After that, they are deleted and no longer appear in the list\.

**To accept or decline an invitation \(AWS CLI, AWS API\)**  
You can use the following commands to accept or decline an invitation:
+ AWS CLI: [aws organizations accept\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/accept-handshake.html), [aws organizations decline\-handshake](https://docs.aws.amazon.com/cli/latest/reference/organizations/decline-handshake.html) 
+ AWS API: [AcceptHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AcceptHandshake.html), [DeclineHandshake](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeclineHandshake.html)