# AWS Config and AWS Organizations<a name="services-that-can-integrate-config"></a>

Multi\-account, multi\-region data aggregation in AWS Config enables you to aggregate AWS Config data from multiple accounts and AWS Regions into a single account\. Multi\-account, multi\-region data aggregation is useful for central IT administrators to monitor compliance for multiple AWS accounts in the enterprise\. An aggregator is a resource type in AWS Config that collects AWS Config data from multiple source accounts and Regions\. Create an aggregator in the Region where you want to see the aggregated AWS Config data\. While creating an aggregator, you can choose to add either individual account IDs or your organization\. For more information about AWS Config, see the [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/)\.

You can also use [AWS Config APIs](https://docs.aws.amazon.com/config/latest/APIReference/welcome.html) to manage AWS Config rules across all AWS accounts in your organization\. For more information, see [Enabling AWS Config Rules Across All Accounts in Your Organization](https://docs.aws.amazon.com/config/latest/developerguide/config-rule-multi-account-deployment.html) in the *AWS Config Developer Guide*\.

Use the following information to help you to help you integrate AWS Config with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-config)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-config)
+ [Enabling trusted access with AWS Config](#integrate-enable-ta-config)
+ [Disabling trusted access with AWS Config](#integrate-disable-ta-config)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-config"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Config to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Config and Organizations or if the account is removed from the organization\.
+ For AWS Config: `AWSConfigRoleForOrganizations`
+ For AWS Config rules: `AWSServiceRoleForConfigMultiAccountSetup` 

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-config"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS Config grant access to the following service principals:
+ For AWS Config: `config.amazonaws.com`
+ For AWS Config rules: `config-multiaccountsetup.amazonaws.com`

## Enabling trusted access with AWS Config<a name="integrate-enable-ta-config"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

**To enable trusted access using the AWS Config console**  
To enable trusted access to AWS Organizations using AWS Config, create a multi\-account aggregator and add the organization\. For information on how to configure a multi\-account aggregator, see [Setting up an aggregator using the console](https://docs.aws.amazon.com/config/latest/developerguide/setup-aggregator-console.html) in the *AWS Config Developer Guide*\.

## Disabling trusted access with AWS Config<a name="integrate-disable-ta-config"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

On the Organizations side, you can disable trusted access by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Config as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle config.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------