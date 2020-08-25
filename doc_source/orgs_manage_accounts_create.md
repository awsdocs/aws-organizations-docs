# Creating an AWS account in your organization<a name="orgs_manage_accounts_create"></a>

This page describes how to create accounts within your organization in AWS Organizations\. To learn about getting started with AWS and creating a single AWS account, see the [Getting Started Resource Center](https://aws.amazon.com/getting-started/)\.

An organization is a collection of AWS accounts that you centrally manage\. You can perform the following procedures to manage the accounts that are part of your organization:
+ [Creating an AWS account that is part of your organization](#orgs_manage_accounts_create-new)
+ [Accessing a member account that has a master account access role](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)

**Important**  
When you create a member account in your organization, AWS Organizations automatically creates an IAM role in the member account that enables IAM users in the master account to exercise full administrative control over the member account\. This role is subject to any [service control policies \(SCPs\)](orgs_manage_policies_scps.md) that apply to the member account\.  
AWS Organizations also automatically creates a service\-linked role named `AWSServiceRoleForOrganizations` that enables integration with select AWS services\. You must configure the other services to allow the integration\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.
If this organization is managed with AWS Control Tower, then create your accounts by using the AWS Control Tower account factory in the AWS Control Tower console or APIs\. If you create the account in Organizations then that account isn't enrolled with AWS Control Tower\. For more information, see [Referring to Resources Outside of AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/external-resources.html#ungoverned-resources) in the *AWS Control Tower User Guide*\.

## Creating an AWS account that is part of your organization<a name="orgs_manage_accounts_create-new"></a>

When signed in to the organization's master account, you can create member accounts that are automatically part of your organization\. To do this, complete the following steps\.

**Minimum permissions**  
To create a member account in your organization, you must have the following permissions:  
`organizations:CreateAccount`
`organizations:DescribeOrganization` \(console only\)
`iam:CreateServiceLinkedRole` \(granted to principal `organizations.amazonaws.com` to enable creating the required service\-linked role in the member accounts\)\.

**Important**  
When you create an account using the following procedure, Organizations copies the following information from the master account to the new member account:  
Account name
Phone number
Company name
Customer URL
Company contact email
Communication language 
Marketplace \(vendor of the account in some AWS Regions\)
AWS does ***not*** automatically collect all the information required for an account to operate as a standalone account\. If you ever need to remove the account from the organization and make it a standalone account, you must provide that information for the account before you can remove it\. For more information, see [Leaving an organization as a member account](orgs_manage_accounts_remove.md#orgs_manage_accounts_leave-as-member)\.

**To create an AWS account that automatically is part of your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose **Add account**\.

1. Choose **Create account**\.

1. Enter the name that you want to assign to the account\. This name helps you distinguish the account from all other accounts in the organization and is separate from the IAM alias or the email name of the owner\.

1. Enter the email address for the owner of the new account\. This address must be unique to this account because it can be used to sign in as the root user of the account\.

1. \(Optional\) Specify the name to assign to the IAM role that is automatically created in the new account\. This role grants the organization's master account permission to access the newly created member account\. If you don't specify a name, AWS Organizations gives the role a default name of `OrganizationAccountAccessRole`\. 
**Important**  
Remember this role name\. You need it later to grant access to the new account for IAM users in the master account\.

1. Choose **Create**\.
**Important**  
If you get an error that indicates that you exceeded your account limits for the organization, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.
If you get an error that indicates that you can't add an account because your organization is still initializing, wait one hour and try again\.
You can also check the AWS CloudTrail log for information on whether the account creation was successful\. For more information, see [Logging and monitoring in AWS Organizations](orgs_security_incident-response.md)\.
If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

1. You are redirected to the **Accounts**/**All accounts** tab, showing your new account at the top of the list with its status set to **Pending creation**\. When the account is created, this status changes to **Active**\. 
**Note**  
By default, the **Accounts** tab hides account creation requests that failed\. To show them, choose the switch at the top of the list and change it to **Show**\.

1. Now that the account exists and has an IAM role that grants administrator access to users in the master account, you can access the account by following the steps in [Accessing and administering the member accounts in your organization](orgs_manage_accounts_access.md)\.

   When you create an account, AWS Organizations initially assigns a password to the root user that is a minimum of 64 characters long\. All characters are randomly generated with no guarantees on the appearance of certain character sets\. You can't retrieve this initial password, as it's discarded after the account is created\. To access the account as the root user for the first time, you must go through the process for password recovery\. For more information, see [Accessing a member account as the root user](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

**To create an AWS account that automatically is part of your organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to create an account:
+ AWS CLI: [aws organizations create\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-account.html)
+ AWS API: [CreateAccount](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateAccount.html)