# Managing the AWS Accounts in Your Organization<a name="orgs_manage_accounts"></a>

An organization is a collection of AWS accounts that you centrally manage\. You can perform the following tasks to manage the accounts that are part of your organization:

+ View details of the accounts in your organization\. You can see the account's unique ID number, its Amazon Resource Name \(ARN\), and the policies that are attached to it\.

+ Invite existing AWS accounts to join your organization\. Create invitations, manage invitations that you have created, and accept or decline invitations\.

+ Create an AWS account as part of your organization\. Create and access an AWS account that is automatically part of your organization\.

+ Remove an AWS account from your organization\. As an administrator in the master account, remove member accounts that you no longer want to manage from your organization\. As an administrator of a member account, remove your account from its organization\. Note that if the master account has attached a policy to your member account, you could be blocked from removing your account\. 

+ Delete \(or close\) an AWS account\. When you no longer need an AWS account, you can close the account to prevent any usage or accrual of charges\.

**Important**  
There are important differences between adding accounts to your organization by *inviting* existing accounts versus *creating* accounts:  
When you create a member account, AWS automatically configures the account with an IAM role that gives the master account administrative control of the member account\. The master account is essentially the owner of the created account\. Created accounts do not have to approve the change from supporting only consolidated billing features to supporting all features in the organization\.
When you invite an account to join your organization and your invitation is accepted, the invited account is not automatically configured with a role that grants the master account administrative control of the invited account\. If you want to enable administrative control, you can manually add the IAM role to the member account\. For more information, see [Creating the OrganizationAccountAccessRole in an Invited Member Account](orgs_manage_accounts_access.md#orgs_manage_accounts_create-cross-account-role)\. 
If you invite an account to join an organization that has enabled only the consolidated billing features rather than all features, and you later want to enable all features for the organization, the invited account must approve the change\.