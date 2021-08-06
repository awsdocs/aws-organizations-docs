# Removing a member account from your organization<a name="orgs_manage_accounts_remove"></a>

Part of managing accounts in an organization is removing *member* accounts that you no longer need\. This page describes what you need to know before removing an account and provides procedures for removing accounts\.

For information on removing the *management account*, see [Deleting the organization by removing the management account](orgs_manage_org_delete.md)\.

**Topics**
+ [Before removing an account from an organization](#orgs_manage_account-before-remove)
+ [Removing a member account from your organization](#orgs_manage_accounts_remove-member-account)
+ [Leaving an organization as a member account](#orgs_manage_accounts_leave-as-member)

## Before removing an account from an organization<a name="orgs_manage_account-before-remove"></a>

Before you remove an account, it's important to know the following:
+ You can remove an account from your organization only if the account has the information that is required for it to operate as a standalone account\. When you create an account in an organization using the AWS Organizations console, API, or AWS CLI commands, all the information that is required of standalone accounts is not automatically collected\. For each account that you want to make standalone, you must choose a support plan, provide and verify the required contact information, and provide a current payment method\. AWS uses the payment method to charge for any billable \(not AWS Free Tier\) AWS activity that occurs while the account isn't attached to an organization\.
+ To remove an account that you created in the organization, you must wait until at least seven days after the account was created\. Invited accounts aren't subject to this waiting period\. 
+ At the moment the account successfully leaves the organization, the owner of the AWS account becomes responsible for all new AWS costs accrued, and the account's payment method is used\. The management account of the organization is no longer responsible\.
+ The account that you want to remove must not be a delegated administrator account for any AWS service enabled for your organization\. If the account is a delegated administrator, you must first change the delegated administrator account to another account that is remaining in the organization\. For more information about how to disable or change the delegated administrator account for an AWS service, see the documentation for that service
+ Even after the removal of created accounts \(accounts created using the AWS Organizations console or the `CreateAccount` API\) from within an organization, \(i\) created accounts are governed by the terms of the creating management account's agreement with us, and \(ii\) the creating management account remains jointly and severally liable for any actions taken by its created accounts\. Customers' agreements with us, and the rights and obligations under those agreements, cannot be assigned or transferred without our prior consent\. To obtain our consent, contact us at [https://aws\.amazon\.com/contact\-us/](https://aws.amazon.com/contact-us/)\.
+ When a member account leaves an organization, that account no longer has access to cost and usage data from the time range when the account was a member of the organization\. However, the management account of the organization can still access the data\. If the account rejoins the organization, the account can access that data again\.
+ When a member account leaves an organization, all tags attached to the account are deleted\.

### Effects of removing an account from an organization<a name="orgs_manage_account-remove-affects"></a>

When you remove an account from an organization, no direct changes are made to the account\. However, the following indirect effects occur:
+ The account is now responsible for paying its own charges and must have a valid payment method attached to the account\.
+ The principals in the account are no longer affected by any [policies](orgs_manage_policies.md) that applied in the organization\. This means that restrictions imposed by SCPs are gone, and the users and roles in the account might have more permissions than they had before\. Other organization policy types can no longer enforced or processed\. 
+ If you use the `aws:PrincipalOrgID` condition key in any policies to restrict access to only users and roles from AWS accounts in your organization, then you should review, and possibly update these policies before removing the member account\. If you don't update the policies, then users and roles in the account could lose access to the resources when the account leaves the organization\.
+ Integration with other services might be disabled\. If you remove an account from an organization that has integration with an AWS service enabled, the users in that account can no longer use that service\.

## Removing a member account from your organization<a name="orgs_manage_accounts_remove-member-account"></a>

When you sign in to the organization's management account, you can remove member accounts from the organization that you no longer need\. To do this, complete the following procedure\. These procedures apply only to member accounts\. To remove the management account, you must [delete the organization](orgs_manage_org_delete.md)\.

**Note**  
If a member account is removed from an organization, that member account will no longer be covered by organization agreements\. Management account administrators should communicate this to member accounts before removing member accounts from the organization, so that member accounts can put new agreements in place if necessary\. A list of active organization agreements can be viewed in the AWS Artifact console on the [AWS Artifact Organization Agreements](https://console.aws.amazon.com/artifact/home?#!/agreements?tab=organizationAgreements) page\.

**Minimum permissions**  
To remove one or more member accounts from your organization, you must sign in as an IAM user or role in the management account with the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:RemoveAccountFromOrganization` 
If you choose to sign in as an IAM user or role in a member account in step 6, then that user or role must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console\.
`organizations:LeaveOrganization` – Note that the organization administrator can apply a policy to your account that removes this permission, preventing you from removing your account from the organization\.
If you sign in as an IAM user and the account is missing payment information, the IAM user must have the permissions `aws-portal:ModifyBilling` and `aws-portal:ModifyPaymentMethods`\. Also, the member account must have IAM user access to billing enabled\. If this isn't already enabled, see [Activating Access to the Billing and Cost Management Console](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\.

------
#### [ AWS Management Console ]

**To remove a member account from your organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, find and choose the check box![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/checkbox-selected.png) next to each member account that you want to remove from your organization\. You can navigate the OU hierarchy or enable **View AWS accounts only** to see a flat list of accounts without the OU structure\. If you have a lot of accounts, you might have to choose **Load more accounts in '*ou\-name*'** at the bottom of the list to find all of those you want to move\.

   On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, find and choose the name of the member account that you want to remove from your organization\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the account that you want\.

1. Choose **Actions**, then under **AWS account**, choose **Remove from organization**\.

1. In the **Remove account '*account\-name*' \(\#*account\-id\-num*\) from organization?** dialog box, choose **Remove account**\.

1. If AWS Organizations fails to remove one or more of the accounts, it's typically because you have not provided all the required information for the account to operate as a standalone account\. Perform the following steps:

   1. Sign in to the failed accounts\. We recommend that you sign in to the member account by choosing **Copy link**, and then pasting it into the address bar of a new incognito browser window\. If you don't use an incognito window, you're signed out of the management account and won't be able to navigate back to this dialog box\.

   1. The browser takes you directly to the sign\-up process to complete any steps that are missing for this account\. Complete all the steps presented\. They might include the following:
      + Provide contact information
      + Provide a valid payment method
      + Verify the phone number
      + Select a support plan option

   1. After you complete the last sign\-up step, AWS automatically redirects your browser to the AWS Organizations console for the member account\. Choose **Leave organization**, and then confirm your choice in the confirmation dialog box\. You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.

------
#### [ AWS CLI & AWS SDKs ]

**To remove a member account from your organization**  
You can use one of the following commands to remove a member account:
+ AWS CLI: [remove\-account\-from\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/remove-account-from-organization.html)

  ```
  $ aws organizations remove-account-from-organization \
      --account-id 123456789012
  ```

  This command produces no output when successful\.
+ AWS SDKs: [RemoveAccountFromOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_RemoveAccountFromOrganization.html)

------

## Leaving an organization as a member account<a name="orgs_manage_accounts_leave-as-member"></a>

When you sign in to a member account, you can remove that one account from its organization\. To do this, complete the following procedure\. The management account can't leave the organization using this technique\. To remove the management account, you must [delete the organization]()\.

The account that you want to remove must not be a delegated administrator account for any AWS service enabled for your organization\. If the account is a delegated administrator, you must first change the delegated administrator account to another account that is remaining in the organization\. For more information about how to disable or change the delegated administrator account for an AWS service, see the documentation for that service

**Important**  
If you leave an organization, you are no longer covered by organization agreements that were accepted on your behalf by the management account of the organization\. You can view a list of these organization agreements in the AWS Artifact console on the [AWS Artifact Organization Agreements](https://console.aws.amazon.com/artifact/home?#!/agreements?tab=organizationAgreements) page\. Before leaving the organization, you should determine \(with the assistance of your legal, privacy, or compliance teams where appropriate\) whether it is necessary for you to have new agreement\(s\) in place\.

**Note**  
An account’s status with an organization affects what cost and usage data is visible:  
If a member account leaves an organization and becomes a standalone account, the account no longer has access to cost and usage data from the time range when the account was a member of the organization\. The account has access only to the data that is generated as a standalone account\. 
If a member account leaves organization A to join organization B, the account no longer has access to cost and usage data from the time range when the account was a member of organization A\. The account has access only to the data that is generated as a member of organization B\. 
If an account rejoins an organization that it previously belonged to, the account regains access to its historical cost and usage data\.

**Minimum permissions**  
To leave an AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console\.
`organizations:LeaveOrganization` – Note that the organization administrator can apply a policy to your account that removes this permission, preventing you from removing your account from the organization\.
If you sign in as an IAM user and the account is missing payment information, the IAM user must have the permissions `aws-portal:ModifyBilling` and `aws-portal:ModifyPaymentMethods`\. Also, the member account must have IAM user access to billing enabled\. If this isn't already enabled, see [Activating Access to the Billing and Cost Management Console](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\. 

------
#### [ AWS Management Console ]

**To leave an organization as a member account**

1. Sign in to the AWS Organizations console at [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in a member account\. 

   By default, you don't have access to the root user password in a member account that was created using AWS Organizations\. If required, recover the root user password by following the steps at [Accessing a member account as the root user](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. On the **[Organizations Dashboard](https://console.aws.amazon.com/organizations/v2/home/dashboard)** page, choose **Leave organization**\.

1. Perform one of the following steps:
   + If your account has all the required information to operate as a standalone account, a confirmation dialog box appears\. Confirm your choice to remove the account\. You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.
   + If your account doesn't have all the required information, perform the following steps:

     1. A dialog box appears to explain that you must complete some additional steps\. Click the link\.

     1. Complete all the sign\-up steps that are presented\. They might include the following:
        + Provide contact information
        + Provide a valid payment method
        + Verify the phone number
        + Select a support plan option

     1. When you see the dialog box stating that the sign\-up process is complete, choose **Leave organization**\.

     1. A confirmation dialog box appears\. Confirm your choice to remove the account\. You are redirected to the **Getting Started** page of the AWS Organizations console, where you can view any pending invitations for your account to join other organizations\.

1. Remove the IAM roles that grant access to your account from the organization\.
**Important**  
If your account was created in the organization, then Organizations automatically created an IAM role in the account that enabled access by the organization's management account\. If the account was invited to join, then Organizations did not automatically create such a role, but you or another administrator might have created one to get the same benefits\. In either case, when you remove the account from the organization, any such role isn't automatically deleted\. If you want to terminate this access from the former organization's management account, then you must manually delete this IAM role\. For information about how to delete a role, see [Deleting roles or instance profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html) in the *IAM User Guide*\.

------
#### [ AWS CLI & AWS SDKs ]

**To leave an organization as a member account**  
You can use one of the following commands to leave an organization:
+ AWS CLI: [leave\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/leave-organization.html)

  The following example causes the account whose credentials are used to run the command to leave the organizatio\.\.

  ```
  $ aws organizations leave-organization
  ```

  This command produces no output when successful\.
+ AWS SDKs: [LeaveOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_LeaveOrganization.html)

------