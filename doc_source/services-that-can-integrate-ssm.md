# AWS Systems Manager and AWS Organizations<a name="services-that-can-integrate-ssm"></a>

AWS Systems Manager is a collection of capabilities that enable visibility and control of your AWS resources\. One of these capabilities, Systems Manager Explorer, is a customizable operations dashboard that reports information about your AWS resources\. You can synchronize operations data across all AWS accounts in your organization by using Organizations and Systems Manager Explorer\. For more information, see [Systems Manager Explorer](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer.html) in the *AWS Systems Manager User Guide*\.

Use the following information to help you to help you integrate AWS Systems Manager with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-ssm)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-ssm)
+ [Enabling trusted access with Systems Manager](#integrate-enable-ta-ssm)
+ [Disabling trusted access with Systems Manager](#integrate-disable-ta-ssm)
+ [Enabling a delegated administrator account for Systems Manager](#integrate-disable-da-ssm)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-ssm"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow Systems Manager to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Systems Manager and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForAmazonSSM_AccountDiscovery`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-ssm"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Systems Manager grant access to the following service principals:
+ `ssm.amazonaws.com`

## Enabling trusted access with Systems Manager<a name="integrate-enable-ta-ssm"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You must sign in with your AWS Organizations management account and create a Resource Data Sync\. For information, see [Setting Up Explorer to Display Data from Multiple Accounts and Regions](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer-resource-data-sync.html) in the *AWS Systems Manager User Guide*\.

## Disabling trusted access with Systems Manager<a name="integrate-disable-ta-ssm"></a>

Systems Manager requires trusted access with AWS Organizations to synchronize operations data across AWS accounts in your organization\. If you disable trusted access, then Systems Manager fails to synchronize operations data and reports an error\.

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

**To disable trusted access using the Systems Manager console**  
See [Deleting a Systems Manager Explorer Resource Data Sync](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer-using-resource-data-sync-delete.html) in the *AWS Systems Manager User Guide*\. To reenable trusted access, you must create a new Resource Data Sync for Systems Manager Explorer\.

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. If you are the administrator of only AWS Organizations and not AWS Systems Manager, wait until the administrator of AWS Systems Manager tells you that they disabled integration with that service's console or tools, and that any resources have been cleaned up\.

1. In the **Trusted access for AWS services** section, find the entry for **AWS Systems Manager**, and then choose **Disable access**\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Systems Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access --service-principle ssm.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for Systems Manager<a name="integrate-disable-da-ssm"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for Systems Manager that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Systems Manager\.

**To designate a member account as a delegated administrator for Systems Manager**  
See [Configuring a Delegated Administrator](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer-setup-delegated-administrator.html) in the *AWS Systems Manager User Guide*\.