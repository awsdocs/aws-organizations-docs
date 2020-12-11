# Amazon GuardDuty and AWS Organizations<a name="services-that-can-integrate-guardduty"></a>

Amazon GuardDuty is a continuous security monitoring service that analyzes and processes a variety data sources, using threat intelligence feeds and machine learning to identify unexpected and potentially unauthorized and malicious activity within your AWS environment\. This can include issues like escalations of privileges, uses of exposed credentials, or communication with malicious IP addresses, URLs, or domains\. 

You can help simplify management of GuardDuty by using Organizations to manage GuardDuty across all of the accounts in your organization\.

For more information, see [Managing GuardDuty accounts with AWS Organizations](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.htm) in the *Amazon GuardDuty User Guide*

Use the following information to help you to help you integrate Amazon GuardDuty with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-guardduty)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-guardduty)
+ [Enabling trusted access with GuardDuty](#integrate-enable-ta-guardduty)
+ [Disabling trusted access with GuardDuty](#integrate-disable-ta-guardduty)
+ [Enabling a delegated administrator account for GuardDuty](#integrate-enable-da-guardduty)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-guardduty"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow GuardDuty to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between GuardDuty and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForAmazonGuardDuty`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-guardduty"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by GuardDuty grant access to the following service principals:
+ `guardduty.amazonaws.com`

## Enabling trusted access with GuardDuty<a name="integrate-enable-ta-guardduty"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using the Amazon GuardDuty console or the AWS Organizations console\.

Amazon GuardDuty requires trusted access to AWS Organizations before you can designate a member account to be the GuardDuty administrator for your organization\. If you configure a delegated administrator using the AWS Management Console, then GuardDuty automatically enables trusted access for you\. However, if you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, then you must explicitly call the [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html) operation and provide the service principal as a parameter\. Then you can call [EnableOrganizationAdminAccount](https://docs.aws.amazon.com/guardduty/latest/APIReference/API_EnableOrganizationAdminAccount.html) to delegate the GuardDuty administrator account\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find the row for **Amazon GuardDuty** and then choose **Enable access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Amazon GuardDuty that they can now enable that service to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable Amazon GuardDuty as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle guardduty.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with GuardDuty<a name="integrate-disable-ta-guardduty"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access only by using the AWS Organizations console\. 

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. If you are the administrator of only AWS Organizations and not Amazon GuardDuty, wait until the administrator of Amazon GuardDuty tells you that they disabled integration with that service's console or tools, and that any resources have been cleaned up\.

1. In the **Trusted access for AWS services** section, find the entry for **Amazon GuardDuty**, and then choose **Disable access**\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable Amazon GuardDuty as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle guardduty.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for GuardDuty<a name="integrate-enable-da-guardduty"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for GuardDuty that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of GuardDuty\.

**Minimum permissions**  
For information about the permissions required to designate a member account as a delegated administrator, see [Permissions required to designate a delegated administrator](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.html#organizations_permissions) in the *Amazon GuardDuty User Guide*

**To designate a member account as a delegated administrator for GuardDuty**  
See [Designate a delegated administrator and add member accounts \(console\)](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.html#organization_thru_console) and [Designate a delegated administrator and add member accounts \(API\)](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.html#organization_thru_api)