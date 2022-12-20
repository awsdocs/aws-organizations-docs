# Amazon DevOps Guru and AWS Organizations<a name="services-that-can-integrate-devops"></a>

Amazon DevOps Guru analyzes operational data and application metrics and events to identify behaviors that deviate from normal operating patterns\. Users are notified when DevOps Guru detects an operational issue or risk\. 

Using DevOps Guru enables multi\-account support with AWS Organizations, so you can designate a member account to manage insights across your entire organization\. This delegated administrator can then view, sort, and filter insights from all accounts within your organization to develop a holistic view of the health of all monitored applications within your organization without the need for any additional customization\.

For more information, see [Monitor accounts across your organization](https://docs.aws.amazon.com/devops-guru/latest/userguide/getting-started-multi-account.html) in the *Amazon DevOps Guru User Guide*\. 

Use the following information to help you integrate Amazon DevOps Guru with AWS Organizations\. 

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-devops"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows DevOps Guru to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between DevOps Guru and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForDevOpsGuru`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-devops"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by DevOps Guru grant access to the following service principals:
+ `devops-guru.amazonaws.com` 

For more information, see [Using service\-linked roles for DevOps Guru](https://docs.aws.amazon.com/devops-guru/latest/userguide/using-service-linked-roles.html) in the *Amazon DevOps Guru User Guide*\. 

## To enable trusted access with DevOps Guru<a name="integrate-enable-ta-devops"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

**Note**  
When you designate a delegated administrator for Amazon DevOps Guru, DevOps Guru automatically enables trusted access for DevOps Guru for your organization\.  
DevOps Guru requires trusted access to AWS Organizations before you can designate a member account to be the delegated administrator for this service for your organization\.

**Important**  
We strongly recommend that whenever possible, you use the Amazon DevOps Guru console or tools to enable integration with Organizations\. This lets Amazon DevOps Guru perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by Amazon DevOps Guru\. For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\. 

You can enable trusted access by using either the AWS Organizations console or the DevOps Guru console\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **Amazon DevOps Guru**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Amazon DevOps Guru that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ DevOps Guru console ]

**To enable trusted service access using the DevOps Guru console**

1. Sign in as administrator in the management account and open DevOps Guru console: [Amazon DevOps Guru console](https://console.aws.amazon.com/devops-guru/management-account) 

1. Choose **Enable trusted access**\.

------

## To disable trusted access with DevOps Guru<a name="integrate-disable-ta-devops"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with Amazon DevOps Guru\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by using the AWS Organizations console\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **Amazon DevOps Guru** and then choose the service’s name\.

1. Choose **Disable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Amazon DevOps Guru that they can now disable that service using its console or tools from working with AWS Organizations\.

------

## Enabling a delegated administrator account for DevOps Guru<a name="integrate-enable-da-devops"></a>

The delegated administrator account for DevOps Guru can see the insights data from all the member accounts which are onboarded to DevOps Guru from the organization\. For information on how a delegated administrator manages organization accounts, see [Monitor accounts across your organization](https://docs.aws.amazon.com/devops-guru/latest/userguide/getting-started-multi-account.html) in the *Amazon DevOps Guru User Guide*\. 

Only an administrator in the organization management account can configure a delegated administrator for DevOps Guru\.

You can specify a delegated administrator account from the DevOps Guru console, or by using the Organizations `RegisterDelegatedAdministrator` CLI or SDK operation\. 

**Minimum permissions**  
Only an IAM user or role in the Organizations management account can configure a member account as a delegated administrator for DevOps Guru in the organization

------
#### [ DevOps Guru console ]

**To configure a delegated administrator in the DevOps Guru console**

1. Sign in as administrator in the management account and open DevOps Guru console: [Amazon DevOps Guru console](https://console.aws.amazon.com/devops-guru/management-account) 

1. Choose **Register delegated administrator**\. You can choose either Management account or any member account as the delegated admin\.

------
#### [ AWS CLI, AWS API ]

If you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, you can use the following commands:
+ AWS CLI: 

  ```
  $ aws organizations register-delegated-administrator \
      --account-id 123456789012 \
      --service-principal devops-guru.amazonaws.com
  ```
+ AWS SDK: Call the Organizations `RegisterDelegatedAdministrator` operation and the member account's ID number and identify the account service `principal account.amazonaws.com` as parameters\. 

------

## Disabling a delegated administrator for DevOps Guru<a name="integrate-disable-da-devops"></a>

 You can remove the delegated administrator using either the DevOps Guru console, or by using the Organizations `DeregisterDelegatedAdministrator` CLI or SDK operation\. For information on how to remove a delegated administrator using the DevOps Guru console, see [Monitor accounts across your organization](https://docs.aws.amazon.com/devops-guru/latest/userguide/getting-started-multi-account.html) in the *Amazon DevOps Guru User Guide*\. 