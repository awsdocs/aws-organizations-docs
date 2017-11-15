# Creating an AWS Account in Your Organization<a name="orgs_manage_accounts_create"></a>

An organization is a collection of AWS accounts that you centrally manage\. You can perform the following procedures to manage the accounts that are part of your organization:

+ [Creating an AWS Account That Is Part of Your Organization](#orgs_manage_accounts_create-new)

+ [Accessing a Member Account That Has a Master Account Access Role](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)

**Important**  
When you create a member account in your organization, Organizations automatically creates an IAM role in the member account that enables IAM users in the master account to exercise full administrative control over the member account\. Note that this role is subject to any service control policies \(SCPs\) that apply to the member account\.  
Organizations also automatically creates a service\-linked role named AWSServiceRoleForOrganizations that enables integration with select AWS services\. You must configure the other services to allow the integration\. For more information, see [AWS Organizations and Service\-Linked Roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

## Creating an AWS Account That Is Part of Your Organization<a name="orgs_manage_accounts_create-new"></a>

When signed in to the organization's master account, you can create member accounts that are automatically part of your organization\. To do this, complete the following steps\.

**Minimum permissions**  
To create a member account in your organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:CreateAccount` 

**Important**  
When you create an account using the following procedure, AWS does not automatically collect all the information required for an account to operate as a standalone account\. If you ever need to remove the account from the organization and make it a standalone account, then you must provide that information for the account before you can remove it\. For more information, see [To leave an organization when all required account information has *not* yet been provided \(console\)](orgs_manage_accounts_remove.md#leave-without-all-info)\.

**To create an AWS account that automatically is part of your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose **Add account**\.

1. Choose **Create account**\.

1. Type the name that you want to assign to the account\. This will help you distinguish the account later from all other accounts in the organization, and is separate from the IAM alias or the email name of the owner\.

1. Type the email address for the owner of the new account\. This must be unique to this account, because it can be used to sign in as the root user of the account\.

1. \(Optional\) Specify the name to be assigned to the IAM role that is automatically created in the new account\. This role grants the organization's master account permission to access the newly created member account\. If you do not specify a name, AWS Organizations gives the role a default name of `OrganizationAccountAccessRole`\. 
**Important**  
Remember this role name\. You need it later to grant access to the new account for IAM users in the master account\.

1. Choose **Create**\.
**Important**  
If you get an error that indicates that you exceeded your account limits for the organization or that you can't add an account because your organization is still initializing, contact [AWS Customer Support](https://console.aws.amazon.com/support/home#/)\.

1. You are redirected to the **Accounts**/**All accounts** tab, showing your new account at the top of the list with its status set to **Pending creation**\. This status changes to **Active** when the account is created\. 
**Note**  
By default, the **Accounts** tab hides account creation requests that failed\. To show these, choose the switch at the top of the list and change it to **Show**\.

1. Now that the account exists and has an IAM role that grants admin access to users in the master account, you can access the account by following the steps in [Accessing and Administering the Member Accounts in Your Organization](orgs_manage_accounts_access.md)\.

**To create an AWS account that is automatically part of your organization \(AWS CLI, AWS API\)**  
You can use the following command or operation to create an account:

+ AWS CLI: [aws organizations create\-account](http://docs.aws.amazon.com/cli/latest/reference/organizations/create-account.html)

+ AWS API: [CreateAccount](http://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateAccount.html)