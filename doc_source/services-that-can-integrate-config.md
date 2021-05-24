# AWS Config and AWS Organizations<a name="services-that-can-integrate-config"></a>

Multi\-account, multi\-region data aggregation in AWS Config enables you to aggregate AWS Config data from multiple accounts and AWS Regions into a single account\. Multi\-account, multi\-region data aggregation is useful for central IT administrators to monitor compliance for multiple AWS accounts in the enterprise\. An aggregator is a resource type in AWS Config that collects AWS Config data from multiple source accounts and Regions\. Create an aggregator in the Region where you want to see the aggregated AWS Config data\. While creating an aggregator, you can choose to add either individual account IDs or your organization\. For more information about AWS Config, see the [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/)\.

You can also use [AWS Config APIs](https://docs.aws.amazon.com/config/latest/APIReference/welcome.html) to manage AWS Config rules across all AWS accounts in your organization\. For more information, see [Enabling AWS Config Rules Across All Accounts in Your Organization](https://docs.aws.amazon.com/config/latest/developerguide/config-rule-multi-account-deployment.html) in the *AWS Config Developer Guide*\.

Use the following information to help you integrate AWS Config with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-config"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Config to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Config and Organizations, or if you remove the member account from the organization\.
+ For AWS Config: `AWSConfigRoleForOrganizations`
+ For AWS Config rules: `AWSServiceRoleForConfigMultiAccountSetup` 

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-config"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS Config grant access to the following service principals:
+ For AWS Config: `config.amazonaws.com`
+ For AWS Config rules: `config-multiaccountsetup.amazonaws.com`

## Enabling trusted access with AWS Config<a name="integrate-enable-ta-config"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Config console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Config console or tools to enable integration with Organizations\. This lets AWS Config perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Config\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Config console or tools then you don’t need to complete these steps\.

**To enable trusted access using the AWS Config console**  
To enable trusted access to AWS Organizations using AWS Config, create a multi\-account aggregator and add the organization\. For information on how to configure a multi\-account aggregator, see [Setting up an aggregator using the console](https://docs.aws.amazon.com/config/latest/developerguide/setup-aggregator-console.html) in the *AWS Config Developer Guide*\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Config**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Config that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Config as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal config.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Config<a name="integrate-disable-ta-config"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Config as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal config.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------