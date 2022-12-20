# AWS Account Management and AWS Organizations<a name="services-that-can-integrate-account"></a>

AWS Account Management helps you manage the account information and metadata for all of the AWS accounts in your organization\. You can set, modify, or delete the alternate contact information for each of your organization's member accounts\.

For more information, see [Using AWS Account Management in your organization](https://docs.aws.amazon.com/accounts/latest/reference/using-orgs.html) in the *AWS Account Management User Guide*\. 

Use the following information to help you integrate AWS Account Management with AWS Organizations\.



## To enable trusted access with Account Management<a name="integrate-enable-ta-account"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

Account Management requires trusted access to AWS Organizations before you can designate a member account to be the delegated administrator for this service for your organization\.

You can enable trusted access using only the Organizations tools\.

You can enable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Account Management as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \
      --service-principal account.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## To disable trusted access with Account Management<a name="integrate-disable-ta-account"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with AWS Account Management\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Account Management as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal account.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for Account Management<a name="integrate-enable-da-account"></a>

When you designate a member account to be a delegated administrator for the organization, users and roles from the designated account can manage the AWS account metadata for other member accounts in the organization\. If you don't enable a delegated admin account, then these tasks can be performed only by the organization's management account\. This helps you to separate management of the organization from management of your account details\.

**Minimum permissions**  
Only an IAM user or role in the Organizations management account can configure a member account as a delegated administrator for Account Management in the organization

For instruction about enabling a delegated administrator account for Account Management, see [Enabling a delegated admin account for AWS Account Management](https://docs.aws.amazon.com/audit-manager/latest/userguide/console-settings.html#settings-ao) in the *AWS Account Management Reference Guide*\.

------
#### [ AWS CLI, AWS API ]

If you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, you can use the following commands:
+ AWS CLI: 

  ```
  $  aws organizations register-delegated-administrator \
      --account-id 123456789012 \
      --service-principal account.amazonaws.com
  ```
+ AWS SDK: Call the Organizations `RegisterDelegatedAdministrator` operation and the member account's ID number and identify the account service `principal account.amazonaws.com` as parameters\. 

------