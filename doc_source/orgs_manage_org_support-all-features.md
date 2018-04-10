# Enabling All Features in Your Organization<a name="orgs_manage_org_support-all-features"></a>

AWS Organizations has two available feature sets: [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) and [all features](orgs_getting-started_concepts.md#feature-set-all)\. All organizations support consolidated billing, which provides basic management tools that you can use to centrally manage the accounts in your organization\. If you enable all features, you continue to get all the consolidated billing features plus a set of advanced features such as service control policies \(SCPs\), which give you fine\-grained control over which services and actions that member accounts can access\. 

When you start the process to enable all features, AWS Organizations sends a request to every member account that you *invited* to join your organization\. Every invited account must approve enabling all features by accepting the request\. Only then can you complete the process to enable all features in your organization\. If an account declines the request then you must either remove the account from your organization or resend the request and get it accepted before you can complete the process to enable all features\. Accounts that you *created* using AWS Organizations don't get a request because they don't need to approve the additional control\.

Organizations also verifies that every account has a service\-linked role named AWSServiceRoleForOrganizations\. This role is mandatory in all accounts to enable all features\. If you deleted the role in an invited account then accepting the invitation to enable all features recreates the role\. If you deleted the role in an account that was created using Organizations then that account receives an invitation specifically to recreate that role\. All of these invitations must be accepted for the organization to complete the process of enabling all features\.

**Notes**  
Currently, AWS Organizations offers only one type of policy called a [service control policy \(SCP\)](orgs_getting-started_concepts.md#scp)\.
Currently, you can have only one root in your organization\.
While enabling all features is in progress, you cannot invite accounts to join the organization\. You must wait until the process to enable all features is complete\. Alternatively, you can cancel the process to enable all features, invite the accounts, and then restart the process to enable all features\.
While enabling all features is in progress, you can continue to create accounts within the organization\.

After you enable all features in your organization, you can enable support in the root for one or more policy types, such as [service control policies \(SCPs\)](orgs_getting-started_concepts.md#scp)\. Within the root, you can attach the policies of the enabled types to the roots, OUs, and accounts in your organization\. For more information, see [Managing Organization Policies](orgs_manage_policies.md)\.

**Important**  
After you enable all features in your organization, the master account in the organization can apply policies to all member accounts\.  
An SCP \(a type of policy\) can restrict what users and even administrators can do in affected accounts\. The master account can apply SCPs that can prevent member accounts from leaving the organization\. Ensure that your account administrators are aware of this\.
The migration from consolidated billing features to all features is one\-way\. You can't switch an organization with all features enabled back to consolidated billing features only\.
The master account is not affected by any SCP\. You can't limit what users and roles in the master account can do by applying SCPs\. SCPs affect only member accounts\.

If your organization has only consolidated billing features enabled, then member account administrators can choose to delete the service\-linked role named AWSServiceRoleForOrganizations\. However, when you enable all features in an organization, this role is required and is recreated in all accounts as part of accepting the invitation to enable all features\. For more information about how Organizations uses this role, see [AWS Organizations and Service\-Linked Roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.

## Beginning the Process to Enable All Features<a name="manage-begin-all-features"></a>

When you sign in with permissions to your organization's master account, you can begin the process to enable all features\. To do this, complete the following steps\.

**Minimum permissions**  
To enable all features in your organization, you must have the following permission:  
`organizations:EnableAllFeatures`

**To ask your member accounts to agree to enable all features in the organization**

1. Sign in to the AWS Management Console and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Settings** tab, choose **Begin process to enable all features**\.

1. Acknowledge your understanding that you cannot return to only consolidated billing features after you switch by choosing **Begin process to enable all features**\. AWS Organizations sends a request to every invited \(not created\) account in the organization asking for approval to enable all features in the organization\. If you have any accounts that were created using Organizations and the member account administrator deleted the service\-linked role named AWSServiceRoleForOrganizations, then Organizations sends that account a request to recreate the role\. 

1. To view the status of the requests, choose **View all feature request approval status**\.

   The **All feature request approval status** page shows the current request status for each account in the organization\. Accounts that have agreed to the request have a green check mark and show the **Acceptance** date\. Accounts that have not yet agreed have a yellow exclamation point icon and show the date that the request was sent with a status of **Open**\.
**Note**  
A countdown of 90 days begins when the request is sent to the member accounts\. All accounts must approve the request within that time period or the request expires\. All requests related to this attempt are canceled, and you have to start over with step 2\.
During the time between when you make the request to enable all features and when either all accounts accept or the timeout occurs, all pending invitations for other accounts to join the organization are automatically canceled\. You can't issue new invitations until the process of enabling all features is finished\. 
After you complete the process of enabling all features, you once again can invite accounts to join the organization\. The process does not change, but all invitations include information letting the recipients know that if they accept the invitation they are subject to any applicable policies\.

1. If an account does not approve its request, you can select the account in the **Account progress** page, and then choose **Remove**\. This cancels the request for the selected account and removes that account from the organization, eliminating the blocker to enabling all features\.

1. After all accounts approve the requests, you can finalize the process and enable all features\. You can also immediately finalize the process if your organization does not have any invited member accounts\. Finalizing the process requires only a couple of clicks in the console\. See [Finalizing the Process to Enable All Features](#finalize-migration)\. 

**To ask your invited member accounts to agree to enable all features in the organization \(AWS CLI, AWS API\)**  
You can use the following command or operation to enable all features in an organization: 
+ AWS CLI: [aws organizations enable\-all\-features](http://docs.aws.amazon.com/cli/latest/reference/organizations/enable-all-features.html)
+ AWS API: [EnableAllFeatures](http://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAllFeatures.html)

## Approving the Request to Enable All Features or to Recreate the Service\-Linked Role<a name="manage-approve-all-features-invite"></a>

When signed in with permissions to one of the organization's invited member accounts, you can approve a request from the master account\. If your account was originally invited to join the organization, then the invitation is to enable all features and implicitly includes approval for recreating the AWSServiceRoleForOrganizations role, if needed\. If your account was instead created using AWS Organizations and you deleted the AWSServiceRoleForOrganizations service\-linked role, then you receive an invitation only to recreate the role\. To do this, complete the following steps\.

**Minimum permissions**  
To approve a request to enable all features for your member account, you must have the following permissions:  
`organizations:AcceptHandshake`
`iam:CreateServiceLinkedRole` – required only if the AWSServiceRoleForOrganizations role must be recreated in the member account

**Important**  
If you perform the steps in the following procedure, the master account in the organization can apply policy\-based controls on your member account that can restrict what users and even what you as the administrator can do in your account\. Additionally, the master account can apply a policy that might prevent your account from leaving the organization\.

**To agree to the request to enable all features in the organization \(console\)**

1. Sign in to the AWS Management Console and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Read what accepting the request for all features in the organization means for your account, and then choose **Accept**\. The page continues to show the process as incomplete until all accounts in the organization accept the requests and the administrator of the master account finalizes the process\.

**To agree to the request to enable all features in the organization \(AWS CLI, AWS API\)**  
To agree to the request, you must accept the handshake with `"Action": "APPROVE_ALL_FEATURES"`\.
+ AWS CLI: [aws organizations accept\-handshake](http://docs.aws.amazon.com/cli/latest/reference/organizations/accept-handshake.html)
+ AWS API: [AcceptHandshake](http://docs.aws.amazon.com/organizations/latest/APIReference/API_AcceptHandshake.html)

## Finalizing the Process to Enable All Features<a name="finalize-migration"></a>

After all invited member accounts must approve the request to enable all features\. If there are no invited member accounts in the organization, the **Enable all features progress** page indicates with a green banner that you can finalize the process\.

**Minimum permissions**  
To finalize the process to enable all features for the organization, you must have the following permission:  
`organizations:AcceptHandshake`

**To finalize the process to enable all features \(console\)**

1. Sign in to the AWS Management Console and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Settings** tab, under **ENABLE ALL FEATURES**, choose **View all feature request approval status**\.

1. After all accounts accept the request to enable all features, in the green box at the top of the page, choose **Finalize process to enable all features**, and then in the confirmation dialog box choose **Finalize process to enable all features** again\.

1. The organization now has all features enabled\. The next step is to enable the policy types that you want to use\. For more information, see [Enabling and Disabling a Policy Type on a Root](orgs_manage_policies.md#enable_policies_on_root)\. After that, you can attach policies to administer the accounts in your organization\. For more information, see [Attaching a Policy to Roots, OUs, or Accounts](orgs_manage_policies.md#attach_policy)

**To finalize the process to enable all features \(AWS CLI, AWS API\)**  
To finalize the process, you must accept the handshake with `"Action": "ENABLE_ALL_FEATURES"`\.
+ AWS CLI: [aws organizations accept\-handshake](http://docs.aws.amazon.com/cli/latest/reference/organizations/accept-handshake.html)
+ AWS API: [AcceptHandshake](http://docs.aws.amazon.com/organizations/latest/APIReference/API_AcceptHandshake.html)