# Amazon GuardDuty and AWS Organizations<a name="services-that-can-integrate-guardduty"></a>

Amazon GuardDuty is a continuous security monitoring service that analyzes and processes a variety data sources, using threat intelligence feeds and machine learning to identify unexpected and potentially unauthorized and malicious activity within your AWS environment\. This can include issues like escalations of privileges, uses of exposed credentials, or communication with malicious IP addresses, URLs, or domains\. 

You can help simplify management of GuardDuty by using Organizations to manage GuardDuty across all of the accounts in your organization\.

For more information, see [Managing GuardDuty accounts with AWS Organizations](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.htm) in the *Amazon GuardDuty User Guide*

Use the following information to help you integrate Amazon GuardDuty with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-guardduty"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow GuardDuty to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between GuardDuty and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForAmazonGuardDuty`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-guardduty"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by GuardDuty grant access to the following service principals:
+ `guardduty.amazonaws.com`

## Enabling trusted access with GuardDuty<a name="integrate-enable-ta-guardduty"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using only the Amazon GuardDuty tools\.

You can enable trusted access using the Amazon GuardDuty console\.

Amazon GuardDuty requires trusted access to AWS Organizations before you can designate a member account to be the GuardDuty administrator for your organization\. If you configure a delegated administrator using the AWS Management Console, then GuardDuty automatically enables trusted access for you\. However, if you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, then you must explicitly call the [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html) operation and provide the service principal as a parameter\. Then you can call [EnableOrganizationAdminAccount](https://docs.aws.amazon.com/guardduty/latest/APIReference/API_EnableOrganizationAdminAccount.html) to delegate the GuardDuty administrator account\.

## Disabling trusted access with GuardDuty<a name="integrate-disable-ta-guardduty"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable Amazon GuardDuty as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal guardduty.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for GuardDuty<a name="integrate-enable-da-guardduty"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for GuardDuty that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of GuardDuty\.

**Minimum permissions**  
For information about the permissions required to designate a member account as a delegated administrator, see [Permissions required to designate a delegated administrator](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.html#organizations_permissions) in the *Amazon GuardDuty User Guide*

**To designate a member account as a delegated administrator for GuardDuty**  
See [Designate a delegated administrator and add member accounts \(console\)](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.html#organization_thru_console) and [Designate a delegated administrator and add member accounts \(API\)](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_organizations.html#organization_thru_api)