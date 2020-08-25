# Managing the AWS accounts in your organization<a name="orgs_manage_accounts"></a>

An organization is a collection of AWS accounts that you centrally manage\. You can perform the following tasks to manage the accounts that are part of your organization:
+ [View details of the accounts in your organization](orgs_manage_org_details.md#orgs_view_account)\. You can see the account's unique ID number, its Amazon Resource Name \(ARN\), and the policies that are attached to it\.
+ [Invite existing AWS accounts to join your organization](orgs_manage_accounts_invites.md)\. Create invitations, manage invitations that you have created, and accept or decline invitations\.
+ [Create an AWS account as part of your organization](orgs_manage_accounts_create.md)\. Create and access an AWS account that is automatically part of your organization\.
+ [Remove an AWS account from your organization](orgs_manage_accounts_remove.md)\. As an administrator in the master account, remove member accounts that you no longer want to manage from your organization\. As an administrator of a member account, remove your account from its organization\. If the master account has attached a policy to your member account, you could be blocked from removing your account\. 
+ [Delete \(or close\) an AWS account](orgs_manage_accounts_close.md)\. When you no longer need an AWS account, you can close the account to prevent any usage or accrual of charges\.

## Impact on an AWS account that you invite to join an organization<a name="impact_of_join"></a>

You invite an AWS account to join an organization\. When the owner of the account accepts the invitation, AWS Organizations automatically makes the following changes to the new member account:
+ AWS Organizations creates a service\-linked role called `AWSServiceRoleForOrganizations`\. The account must have this role if your organization supports all features\. You can delete the role if the organization supports only the consolidated billing feature set\. If you delete the role and later you enable all features in your organization, AWS Organizations recreates the role for the account\. 
+ You might have [service control policies \(SCPs\)](orgs_manage_policies_scps.md) or [tag policies](orgs_manage_policies_tag-policies.md) that are attached to the organization root or the OU that contains the account\. If so, those policies immediately apply to all users and roles in the invited account\.
+ You can [enable service trust for another AWS service](services-that-can-integrate.md) for your organization\. When you do, that trusted service can create service\-linked roles or perform actions in any member account in the organization, including an invited account\.

For invited member accounts, AWS Organizations doesn't automatically create the IAM role [OrganizationAccountAccessRole](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)\. This role grants the master account administrative control of the member account\. If you want to enable that level of administrative control, you can manually add the role to the invited account\. For more information, see [Creating the OrganizationAccountAccessRole in an invited member account](orgs_manage_accounts_access.md#orgs_manage_accounts_create-cross-account-role)\. 

You can invite an account to join an organization that has only the consolidated billing features enabled\. If you later want to enable all features for the organization, invited accounts must approve the change\.

## Impact on an AWS account that you create in an organization<a name="impact_of_create"></a>

When you create an AWS account in your organization, AWS Organizations automatically makes the following changes to the new member account:
+ AWS Organizations creates a service\-linked role called `AWSServiceRoleForOrganizations`\. The account must have this role if your organization supports all features\. You can delete the role if the organization supports only the consolidated billing feature set\. If you delete the role and later you enable all features in your organization, AWS Organizations recreates the role for the account\.
+ AWS Organizations creates the IAM role [OrganizationAccountAccessRole](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)\. This role grants the master account access to the new member account\. This role can be deleted\.
+ If you have any [SCPs attached to the root of the OU tree](orgs_manage_policies_scps.md), those SCPs immediately apply to all users and roles in the created account\. New accounts are added to the root OU by default\.
+ If you have [enabled service trust for another AWS service](services-that-can-integrate.md) for your organization, that trusted service can create service\-linked roles or perform actions in any member account in the organization, including your created account\.