# Removing a Member Account from Your Organization<a name="orgs_manage_accounts_remove"></a>

An organization is a collection of AWS accounts that you centrally manage\. In addition to creating accounts and managing invitations, you can perform the following tasks:
+ [Removing a Member Account from Your Organization](#orgs_manage_accounts_remove-from-master)
+ [Leaving an Organization as a Member Account](#orgs_manage_accounts_leave-as-member)

**Important**  
You can remove an account from your organization only if the account has the information required for it to operate as a standalone account\. When you create an account in an organization using the AWS Organizations console, API, or CLI commands, all the information required of standalone accounts is not automatically collected\. For each account that you want to make standalone, you must accept the AWS Customer Agreement, choose a support plan, provide and verify the required contact information, and provide a current payment method\. AWS uses the payment method to charge for any billable \(not free tier\) AWS activity that occurs while the account is not attached to an organization\. 

**Notes**  
Even after the removal of created accounts \(accounts created using the AWS Organizations console or the `CreateAccount` API\) from within an organization, \(i\) created accounts are governed by the terms of the creating master account’s agreement with us, and \(ii\) the creating master account remains jointly and severally liable for any actions taken by its created accounts\. Customers’ agreements with us, and the rights and obligations under those agreements cannot be assigned or transferred without our prior consent\. To obtain our consent, contact us at [https://aws\.amazon\.com/contact\-us/](https://aws.amazon.com/contact-us/)\.
When a member account leaves an organization, that account no longer has access to cost and usage data from the time range when the account was a member of the organization\. However, the master account of the organization can still access the data\. If the account re\-joins the organization, the account can access that data again\.

## Removing a Member Account from Your Organization<a name="orgs_manage_accounts_remove-from-master"></a>

When you sign in to the organization's master account, you can remove accounts from the organization that you no longer need\. To do this, complete the following procedure\.

**Minimum permissions**  
To remove one or more member accounts from your organization as an IAM user or role in the master account, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:RemoveAccountFromOrganization` 
If you choose to sign\-in as an IAM user or role in a member account in step 6, then that user or role must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:LeaveOrganization` – Note that the organization administrator can apply a policy to your account that removes this permission, preventing you from removing your account from the organization\.
If you sign in as an IAM user and the account is missing payment information, then the IAM user must have the permissions `aws-portal:ModifyBilling` and `aws-portal:ModifyPaymentMethods`\. Also, the member account must have IAM user access to billing enabled\. If this is not already enabled, see [Activating Access to the Billing and Cost Management Console](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\.

**To remove a member account from your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, check the box next to the member account that you want to remove from your organization\. You can choose more than one\.

1. Choose **Remove account**\.

1. In the **Remove account** confirmation dialog box, choose **Remove**\.

1. A dialog box displays the success or failure status for each account\. 

1. If Organizations fails to remove one or more of the accounts, it is typically because you have not provided all the required information for the account to operate as a standalone account\. Perform the following steps:

   1. Choose **Sign in options** for one of the failed accounts\.

   1. We recommend that you sign in to the member account by choosing **Copy link**, and then pasting it into the address bar of a new incognito browser window\. If you don't use an incognito window, then you are signed out of the master account and won't be able to navigate back to this dialog box\.

   1. The browser takes you directly to the sign\-up process to complete any steps that are missing for this account\. Complete all the steps presented\. They might include the following:
      + Provide contact information
      + Accept the AWS Customer Agreement
      + Provide a valid payment method
      + Verify the phone number
      + Select a support plan option

   1. After you complete the last sign\-up step, AWS automatically redirects your browser to the AWS Organizations console for the member account\. Choose **Leave organization**, and then confirm your choice in the confirmation dialog box\. You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.

**To remove a member account from your organization \(AWS CLI, AWS API\)**

You can use the following CLI command or API operation to remove a member account:
+ AWS CLI: [aws organizations remove\-account\-from\-organization](http://docs.aws.amazon.com/cli/latest/reference/organizations/remove-account-from-organization.html)
+ AWS API: [RemoveAccountFromOrganization](http://docs.aws.amazon.com/organizations/latest/APIReference/API_RemoveAccountFromOrganization.html)

## Leaving an Organization as a Member Account<a name="orgs_manage_accounts_leave-as-member"></a>

When signed in to a member account, you can remove that one account from its organization\. To do this, complete the following procedure\.

**Minimum permissions**  
To leave an AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:LeaveOrganization` – Note that the organization administrator can apply a policy to your account that removes this permission, preventing you from removing your account from the organization\.
If you sign in as an IAM user and the account is missing payment information, then the IAM user must have the permissions `aws-portal:ModifyBilling` and `aws-portal:ModifyPaymentMethods`\. Also, the member account must have IAM user access to billing enabled\. If this is not already enabled, see [Activating Access to the Billing and Cost Management Console](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\. 

**To leave an organization as a member account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You can sign in as an IAM user with the required permissions, or as the root user of the member account that you want to remove from the organization\. By default, you don't have access to the root user password in a member account that was created using AWS Organizations\. If required, recover the root user password by following the steps at [Accessing a Member Account as the Root User](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. On the **Organization overview** page, choose **Leave organization**\.

1. Perform one of the following steps:
   + If your account has all the required information to operate as a standalone account, a confirmation dialog box appears\. Confirm your choice to remove the account\. You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.
   + If your account does not have all the required information, then perform the following steps:

     1. A dialog box appears to explain that you must complete some additional steps\. Click the link\.

     1. Complete all the sign\-up steps that are presented\. They might include the following:
        + Provide contact information
        + Accept the AWS Customer Agreement
        + Provide a valid payment method
        + Verify the phone number
        + Select a support plan option

     1. When you see the dialog box stating that the sign\-up process is complete, choose **Leave organization**\.

     1. A confirmation dialog box appears\. Confirm your choice to remove the account\. You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.

**To leave an organization as a member account \(AWS CLI, AWS API\)**

You can use the following CLI command or API operation to leave an organization:
+ AWS CLI: [aws organizations leave\-organization](http://docs.aws.amazon.com/cli/latest/reference/organizations/leave-organization.html)
+ API: [LeaveOrganization](http://docs.aws.amazon.com/organizations/latest/APIReference/API_LeaveOrganization.html)