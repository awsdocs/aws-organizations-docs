# AWS License Manager and AWS Organizations<a name="services-that-can-integrate-license-manager"></a>

AWS License Manager streamlines the process of bringing software vendor licenses to the cloud\. As you build out cloud infrastructure on AWS, you can save costs by using bring\-your\-own\-license \(BYOL\) opportunities—that is, by repurposing your existing license inventory for use with cloud resources\. With rule\-based controls on the consumption of licenses, administrators can set hard or soft limits on new and existing cloud deployments, stopping noncompliant server usage before it happens\.

By linking License Manager with AWS Organizations, you can enable cross\-account discovery of computing resources throughout your organization\.

For more information about License Manager, see the [License Manager User Guide](https://docs.aws.amazon.com/license-manager/latest/userguide/)\.

Use the following information to help you to help you integrate AWS License Manager with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-license-manager)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-license-manager)
+ [Enabling trusted access with License Manager](#integrate-enable-ta-license-manager)
+ [Disabling trusted access with License Manager](#integrate-disable-ta-license-manager)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-license-manager"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow License Manager to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between License Manager and Organizations or if the account is removed from the organization\.
+ `AWSLicenseManagerMasterAccountRole`
+ `AWSLicenseManagerMemberAccountRole`
+ `AWSServiceRoleForAWSLicenseManagerRole`

For more information, see [Using the License Manager–management account role](https://docs.aws.amazon.com/license-manager/latest/userguide/master-role.html) and [Using the License Manager–member account role](https://docs.aws.amazon.com/license-manager/latest/userguide/member-role.html)\.

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-license-manager"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by License Manager grant access to the following service principals:
+ `license-manager.amazonaws.com`
+ `license-manager.member-account.amazonaws.com`

## Enabling trusted access with License Manager<a name="integrate-enable-ta-license-manager"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS License Manager console or the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by using the License Manager console\. This enables License Manager to perform required setup tasks\.

**To enable trusted access with License Manager**  
You must sign in to the License Manager console using your AWS Organizations management account and associate it with your License Manager account\. Then you can configure your License Manager settings\. For information, see [Configuring AWS License Manager Guide Settings](https://docs.aws.amazon.com/license-manager/latest/userguide/settings.html)\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find the row for **AWS License Manager** and then choose **Enable access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS License Manager that they can now enable that service to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS License Manager as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle license-manager.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with License Manager<a name="integrate-disable-ta-license-manager"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

On the Organizations side, you can disable trusted access by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS License Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle license-manager.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------