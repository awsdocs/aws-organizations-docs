# AWS Security Hub and AWS Organizations<a name="services-that-can-integrate-securityhub"></a>

AWS Security Hub provides you with a comprehensive view of your security state in AWS and helps you check your environment against security industry standards and best practices\.

Security Hub collects security data from across your AWS accounts, the AWS services you use, and supported third\-party partner products\. It helps you to analyze your security trends and identify the highest priority security issues\.

When you use both Security Hub and AWS Organizations together, you can automatically enable Security Hub for all of your accounts, including new accounts as they are added\. This increases the coverage for Security Hub checks and findings, which provides a more comprehensive and accurate picture of your overall security posture\.

For more information about Security Hub, see the *[AWS Security Hub User Guide](https://docs.aws.amazon.com/securityhub/latest/userguide/)*\.

Use the following information to help you integrate AWS Security Hub with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-securityhub"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows Security Hub to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between Security Hub and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForSecurityHub`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-securityhub"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Security Hub grant access to the following service principals:
+ `securityhub.amazonaws.com`

## Enabling trusted access with Security Hub<a name="integrate-enable-ta-securityhub"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

When you designate a delegated administrator for Security Hub, Security Hub automatically enables trusted access for Security Hub in your organization\.

## Enabling a delegated administrator account for Security Hub<a name="integrate-enable-da-securityhub"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for Security Hub that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Security Hub\.

For information, see [Designating a Security Hub administrator account](https://docs.aws.amazon.com/securityhub/latest/userguide/designate-orgs-admin-account.html) in the *AWS Security Hub User Guide*\.

**To designate a member account as a delegated administrator for Security Hub**

1. Sign in with your Organizations management account\.

1. Perform one of the following:
   + If your management account does not have Security Hub enabled, then on the Security Hub console, choose **Go to Security Hub**\.
   + If your management account does have Security Hub enabled, then on the Security Hub console, choose **Settings**\.

1. Under **Delegated Administrator**, enter the account ID\.