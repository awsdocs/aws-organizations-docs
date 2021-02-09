# AWS Audit Manager and AWS Organizations<a name="services-that-can-integrate-audit-manager"></a>

AWS Audit Manager helps you continuously audit your AWS usage to simplify how you assess risk and compliance with regulations and industry standards\. Audit Manager automates evidence collection to make it easier to assess if your policies, procedures, and activities are operating effectively\. When it is time for an audit, Audit Manager helps you manage stakeholder reviews of your controls and helps you build audit\-ready reports with much less manual effort\.

When you integrate Audit Manager with AWS Organizations, you can gather evidence from a broader source by including multiple AWS accounts from your organization within the scope of your assessments\.

For more information, see [Enable AWS Organizations](https://docs.aws.amazon.com/audit-manager/latest/userguide/setting-up.html#enabling-orgs) in the *Audit Manager User Guide*\. 

Use the following information to help you to help you integrate AWS Audit Manager with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-audit-manager"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow Audit Manager to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Audit Manager and Organizations, or if you remove the member account from the organization\.

For more information about how Audit Manager uses this role, see [Using service\-linked roles](https://docs.aws.amazon.com/audit-manager/latest/userguide/using-service-linked-roles.html) in the *AWS Audit Manager Users Guide*\.
+ `AWSServiceRoleForAuditManager`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-audit-manager"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Audit Manager grant access to the following service principals:
+ `auditmanager.amazonaws.com`

## To enable trusted access with Audit Manager<a name="integrate-enable-ta-audit-manager"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

Audit Manager requires trusted access to AWS Organizations before you can designate a member account to be the delegated administrator for your organization\.

You can enable trusted access using either the AWS Audit Manager console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Audit Manager console or tools to enable integration with Organizations\. This lets AWS Audit Manager perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Audit Manager\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Audit Manager console or tools then you don’t need to complete these steps\.

**To enable trusted access using the Audit Manager console**  
For instructions about enabling trusted access, see [Setting Up](https://docs.aws.amazon.com/audit-manager/latest/userguide/console-settings.html#settings-ao) in the *AWS Audit Manager User Guide*\.

**Note**  
If you configure a delegated administrator using the AWS Audit Manager console, then AWS Audit Manager automatically enables trusted access for you\.

You can enable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Audit Manager as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \
      --service-principal auditmanager.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## To disable trusted access with Audit Manager<a name="integrate-disable-ta-audit-manager"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with AWS Audit Manager\.

You can disable trusted access using only the Organizations console or tools\.

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Audit Manager** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Audit Manager that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Audit Manager** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Audit Manager that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Audit Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal auditmanager.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for Audit Manager<a name="integrate-enable-da-audit-manager"></a>

When you designate a member account to be a delegated administrator for the organization, users and roles from that account can perform administrative actions for Audit Manager that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Audit Manager\.

**Minimum permissions**  
Only an IAM user or role in the Organizations management account with the following permission can configure a member account as a delegated administrator for Audit Manager in the organization:  
`audit-manager:RegisterAccount`

For instruction about enabling a delegated administrator account for Audit Manager, see [Setting Up](https://docs.aws.amazon.com/audit-manager/latest/userguide/console-settings.html#settings-ao) in the *AWS Audit Manager User Guide*\.

If you configure a delegated administrator using the AWS Audit Manager console, then Audit Manager automatically enables trusted access for you\. 

------
#### [ AWS CLI, AWS API ]

If you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, you can use the following commands:
+ AWS CLI: 

  ```
  $  aws audit-manager register-account \
      --delegated-admin-account 123456789012
  ```
+ AWS SDK: Call the `RegisterAccount` operation and provide `delegatedAdminAccount` as a parameter to delegate the administrator account\. 

------