# AWS License Manager and AWS Organizations<a name="services-that-can-integrate-license-manager"></a>

AWS License Manager streamlines the process of bringing software vendor licenses to the cloud\. As you build out cloud infrastructure on AWS, you can save costs by using bring\-your\-own\-license \(BYOL\) opportunities—that is, by repurposing your existing license inventory for use with cloud resources\. With rule\-based controls on the consumption of licenses, administrators can set hard or soft limits on new and existing cloud deployments, stopping noncompliant server usage before it happens\.

By linking License Manager with AWS Organizations, you can enable cross\-account discovery of computing resources throughout your organization\.

For more information about License Manager, see the [License Manager User Guide](https://docs.aws.amazon.com/license-manager/latest/userguide/)\.

Use the following information to help you integrate AWS License Manager with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-license-manager"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows License Manager to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between License Manager and Organizations, or if you remove the member account from the organization\.
+ `AWSLicenseManagerMasterAccountRole`
+ `AWSLicenseManagerMemberAccountRole`
+ `AWSServiceRoleForAWSLicenseManagerRole`

For more information, see [Using the License Manager–management account role](https://docs.aws.amazon.com/license-manager/latest/userguide/master-role.html) and [Using the License Manager–member account role](https://docs.aws.amazon.com/license-manager/latest/userguide/member-role.html)\.

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-license-manager"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by License Manager grant access to the following service principals:
+ `license-manager.amazonaws.com`
+ `license-manager.member-account.amazonaws.com`

## Enabling trusted access with License Manager<a name="integrate-enable-ta-license-manager"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using only AWS License Manager\.

**To enable trusted access with License Manager**  
You must sign in to the License Manager console using your AWS Organizations management account and associate it with your License Manager account\. For more information, see [Configuring AWS License Manager Guide Settings](https://docs.aws.amazon.com/license-manager/latest/userguide/settings.html)\. It is also summarized here for your convenience\.

**Important**  
This procedure is a one\-way door\. You can't undo this\.

**To enable trusted access between Organizations and License Manager**

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com/) using your organization's management account\.

1. Navigate to the [License Manager console](https://console.aws.amazon.com/license-manager) and choose **Settings**\.

1. Choose **Edit**\.

1. Choose **Link AWS Organizations accounts**\.

## Disabling trusted access with License Manager<a name="integrate-disable-ta-license-manager"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS License Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal license-manager.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for License Manager<a name="integrate-enable-da-license-manager"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for License Manager that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of License Manager\.

To delegate a member account as an administrator for License Manager, follow the steps at [Register a delegated administrator](https://docs.aws.amazon.com/license-manager/latest/userguide/delegated-administrator.html) in the *License Manager User Guide*\. They are also summarized here for your convenience\.

**To register a delegated administrator account for License Manager**

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com/) using your organization's management account\.

1. Navigate to the [License Manager console](https://console.aws.amazon.com/license-manager) and choose **Settings**\.

1. Under **Delegated administrator**, choose **Delegate administrator**\.

1. Enter the account ID number for the AWS account that you want to assign, and then choose **Delegate**\. You can't use the ID for the management account\. It must be a member account\.