# AWS Single Sign\-On and AWS Organizations<a name="services-that-can-integrate-sso"></a>

AWS Single Sign\-On \(AWS SSO\) provides single sign\-on services for all of your AWS accounts and cloud applications\. It connects with Microsoft Active Directory through AWS Directory Service to allow users in that directory to sign in to a personalized user portal using their existing Active Directory user names and passwords\. From the portal, users have access to all the AWS accounts and cloud applications that you provide in the portal\.

For more information about AWS SSO, see the [AWS Single Sign\-On User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/)\.

Use the following information to help you to help you integrate AWS Single Sign\-On with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-sso)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-sso)
+ [Enabling trusted access with AWS SSO](#integrate-enable-ta-sso)
+ [Disabling trusted access with AWS SSO](#integrate-disable-ta-sso)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-sso"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS SSO to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS SSO and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForSSO`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-sso"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS SSO grant access to the following service principals:
+ `sso.amazonaws.com`

## Enabling trusted access with AWS SSO<a name="integrate-enable-ta-sso"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

AWS SSO requires trusted access with AWS Organizations to function\. Trusted access is enabled when you set up AWS SSO\. For more information, see [Getting Started \- Step 1: Enable AWS Single Sign\-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/step1.html) in the *AWS Single Sign\-On User Guide*\.

## Disabling trusted access with AWS SSO<a name="integrate-disable-ta-sso"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

AWS SSO requires trusted access with AWS Organizations to operate\. If you disable trusted access using AWS Organizations while you are using AWS SSO, it stops functioning because it can't access the organization\. Users can't use AWS SSO to access accounts\. Any roles that AWS SSO creates remain, but the AWS SSO service can't access them\. The AWS SSO service\-linked roles remain\. If you reenable trusted access, AWS SSO continues to operate as before, without the need for you to reconfigure the service\. 

If you remove an account from your organization, AWS SSO automatically cleans up any metadata and resources, such as its service\-linked role\. A standalone account that is removed from an organization no longer works with AWS SSO\.