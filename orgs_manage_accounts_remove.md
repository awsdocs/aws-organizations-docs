# Removing a Member Account from Your Organization<a name="orgs_manage_accounts_remove"></a>

An organization is a collection of AWS accounts that you centrally manage\. In addition to creating accounts and managing invitations, you can perform the following tasks:

+ [Removing a Member Account from Your Organization](#orgs_manage_accounts_remove-from-master)

+ [Leaving an Organization as a Member Account](#orgs_manage_accounts_leave-as-member)

When you remove an account from an organization, Organizations automatically attempts to remove the service\-linked role named AWSServiceRoleForOrganizations\. Therefore you must have permissions to remove the service\-linked role in addition to permission to removing the account\.

**Important**  
You can remove an account from your organization only if the account has the information required for it to operate as a standalone account\. When you create an account in an organization using the AWS Organizations console, API, or CLI commands, all the information required of standalone accounts is not automatically collected\. For each account that you want to make standalone, you must accept the AWS Customer Agreement, choose a support plan, provide and verify the required contact information, and provide a current payment method\. AWS uses the payment method to charge for any billable \(not free tier\) AWS activity that occurs while the account is not attached to an organization\. 

## Removing a Member Account from Your Organization<a name="orgs_manage_accounts_remove-from-master"></a>

When signed in to the organization's master account, you can remove accounts from the organization that you no longer need\. To do this, complete the following procedure\.

**Minimum permissions**  
To remove a member account from your organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:RemoveAccountFromOrganization`

**To remove a member account from your organization \(console\)**

The following procedure works only for an account that already has the information required to operate as a standalone AWS account\. If this procedure fails, you must instead follow the steps in the procedure [To leave an organization when all required account information has *not* yet been provided \(console\)](#leave-without-all-info)\. 

1. To remove an account, you must first enable IAM user access to billing in the account\. If the account already has this enabled, move to Step 2\. Otherwise, see [Activating Access to the Billing and Cost Management Console](http://alpha-docs-aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\.

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://alpha-docs-aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose the member account that you want to remove from your organization\. You can choose more than one\.

1. Choose **Remove account**\.

1. In the **Remove account** dialog box, choose **Remove**\.

   The **Account** page is refreshed, and the account that you just removed is no longer listed as part of the organization\.
**Important**  
If the procedure fails at this point then you must instead follow the steps in the procedure [To leave an organization when all required account information has *not* yet been provided \(console\)](#leave-without-all-info)\. 

**To remove a member account from your organization \(AWS CLI, AWS API\)**  
You can use the following CLI command or API operation to remove a member account:

+ AWS CLI: [aws organizations remove\-account\-from\-organization](http://alpha-docs-aws.amazon.com/cli/latest/reference/organizations/remove-account-from-organization.html)

+ AWS API: [RemoveAccountFromOrganization](http://alpha-docs-aws.amazon.com/organizations/latest/APIReference/API_RemoveAccountFromOrganization.html)

## Leaving an Organization as a Member Account<a name="orgs_manage_accounts_leave-as-member"></a>

When signed in to a member account, you can remove that one account from its organization\. To do this, complete the following procedure\.

**Minimum permissions**  
To leave an AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:LeaveOrganization` – Note that the organization administrator can apply a policy to your account that removes this permission, preventing you from removing your account from the organization\.

**To leave an organization when all required account information has already been provided \(console\)**

You typically use the following procedure with accounts that were created outside of Organizations and operated as standalone accounts before being invited to join to an organization\. However, if the account no longer has a current payment method \(that is, the payment method expired after the account joined the organization\), you might instead have to follow the steps in the procedure [To leave an organization when all required account information has *not* yet been provided \(console\)](#leave-without-all-info)\.

1. To leave an organization, you must first enable IAM user access to billing in the account that is leaving\. If the account already has this enabled, move to Step 2\. Otherwise, see [Activating Access to the Billing and Cost Management Console](http://alpha-docs-aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\.

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as the root user of the member account that you want to remove from the organization\. 
**Note**  
By default, you don't have access to the root user password in a member account that was created using AWS Organizations\. If required, recover the root user password by following the steps at [Accessing a Member Account as the Root User](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. On the **Organization overview** page, choose **Leave organization**\.

1. In the **Leave organization** dialog box, choose **Leave organization**\.

   You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.

**To leave an organization when all required account information has *not* yet been provided \(console\)**

To remove your account from an organization so that it can operate as a standalone account, you might first need to provide additional information, such as contact information and a payment method\. Use the following procedure to add the information\. 

1. Contact AWS Support at [https://console\.aws\.amazon\.com/support/home\#/case/create](https://console.aws.amazon.com/support/home#/case/create) to open a support case\. Create the case with the following information:

   + **Regarding:** **Account and Billing**

   + **Service:** **Billing**

   + **Category:** **Consolidated Billing**

   + **Subject:** **Request for help with removing an account from AWS Organizations**

   + **Description:** **Please help me remove the following AWS accounts with missing required information from my organization: *<account IDs>***

   Don't proceed to step 2 until AWS Support confirms that your account is configured for the next steps\.

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as the root user of the member account that you want to remove from the organization\. 
**Note**  
By default, you don't have access to the root user password in a member account that was created with AWS Organizations\. If required, recover the root user password by following the steps at [Accessing a Member Account as the Root User](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. In your browser, navigate to [https://portal.aws.amazon.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Complete all the steps presented\. They might include the following:

   + Accept AWS Customer Agreement

   + Contact information

   + Payment method

   + Phone verification

   + Choose support option

1. After you provide the required information, stay signed in as the root user in the member account and navigate to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. On the **Organization overview** page, choose **Leave organization**\.

1. In the **Leave organization** dialog box, choose **Leave organization**\.

   You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.

1. Contact AWS Support again to inform them that you successfully removed the account from your organization and to close the support case\.

**To leave an organization as a member account \(AWS CLI, AWS API\)**  
You can use the following command or operation to leave an organization:

+ AWS CLI: [aws organizations leave\-organization](http://alpha-docs-aws.amazon.com/cli/latest/reference/organizations/leave-organization.html)

+ API: [LeaveOrganization](http://alpha-docs-aws.amazon.com/organizations/latest/APIReference/API_LeaveOrganization.html)