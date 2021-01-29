# AWS Backup and AWS Organizations<a name="services-that-can-integrate-backup"></a>

AWS Backup is a service that allows you to manage and monitor the AWS Backup jobs in your organization\. Using AWS Backup, if you sign\-in as a user in the organization's management account, you can enable organization\-wide backup protection and monitoring\. It helps you to achieve compliance by using [backup policies](orgs_manage_policies_backup.md) to centrally apply AWS Backup plans to resources across all of the accounts in your organization\. When you use both AWS Backup and AWS Organizations together, you can get the following benefits:

**Protection**  
You can [enable the backup policy type](orgs_manage_policies_enable-disable.md) in your organization and then [create backup policies](orgs_manage_policies_backup_create.md) to attach to the organization's root, OUs, or accounts\. A backup policy combines an AWS Backup plan with the other details required to apply the plan automatically to your accounts\.Policies that are directly attached to an account are merged with policies [inherited](orgs_manage_policies_inheritance_mgmt.md) from the organization's root and any parent OUs to create an [effective policy](orgs_manage_policies_backup_effective.md) that applies to the account\. The policy includes the ID of an IAM role that has permissions to run AWS Backup on the resources in your accounts\. AWS Backup uses the IAM role to perform the backup on your behalf as specified by the backup plan in the effective policy\.

**Monitoring**  
When you [enable trusted access for AWS Backup](orgs_integrate_services.md#orgs_how-to-enable-disable-trusted-access) in your organization, you can use the AWS Backup console to view details about the backup, restore, and copy jobs in any of the accounts in your organization\. For more information, see [Monitor your backup jobs](https://docs.aws.amazon.com/aws-backup/latest/devguide/monitor-and-verify-protected-resources.html) in the *AWS Backup Developer Guide*\.

For more information about AWS Backup, see the *[AWS Backup Developer Guide](https://docs.aws.amazon.com/aws-backup/latest/devguide/)*\.

Use the following information to help you to help you integrate AWS Backup with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-backup)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-backup)
+ [Enabling trusted access with AWS Backup](#integrate-enable-ta-backup)
+ [Disabling trusted access with AWS Backup](#integrate-disable-ta-backup)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-backup"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Backup to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Backup and Organizations or if the account is removed from the organization\.
+ `AWSBackupDefaultServiceRole`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-backup"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS Backup grant access to the following service principals:
+ `backup.amazonaws.com`

## Enabling trusted access with AWS Backup<a name="integrate-enable-ta-backup"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Backup console or the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by using the AWS Backup console\. This enables AWS Backup to perform required setup tasks\.

To enabled trusted access using AWS Backup, see [Enabling Backup in Multiple AWS Accounts](https://docs.aws.amazon.com/aws-backup/latest/devguide/manage-cross-account.html#enable-xaccount-management) in the *AWS Backup Developer Guide*\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We strongly recommend that where possible, you use the AWS Backup console or tools to enable integration with Organizations so that AWS Backup can perform any configuration that it requires\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Backup\.  
If you enable trusted access by using the tools provided by AWS Backup then you don’t need to complete these steps\.

------
#### [ Old console ]

**To enable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Backup**, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, choose **Enable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Backup that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ New console ]

**To enable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Backup**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Backup that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Backup as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal backup.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Backup<a name="integrate-disable-ta-backup"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

AWS Backup requires trusted access with AWS Organizations to enable monitoring of backup, restore, and copy jobs across your organization's accounts\. If you disable trusted access AWS Backup, you lose the ability to view jobs outside of the current account\. The AWS Backup role that AWS Backup creates remains\. If you later re\-enable trusted access, AWS Backup continues to operate as before, without the need for you to reconfigure the service\. 

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We strongly recommend that where possible, you use the AWS Backup console or tools to disable integration with Organizations so that AWS Backup can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Backup, wait until the administrator of AWS Backup tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ Old console ]

**To disable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Backup** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Backup that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Backup** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Backup that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Backup as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal backup.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------