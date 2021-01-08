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

You can enable trusted access using either the AWS Single Sign\-On console or the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by using the AWS SSO console\. This enables AWS SSO to perform required setup tasks\.

AWS SSO requires trusted access with AWS Organizations to function\. Trusted access is enabled when you set up AWS SSO\. For more information, see [Getting Started \- Step 1: Enable AWS Single Sign\-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/step1.html) in the *AWS Single Sign\-On User Guide*\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find the row for **AWS Single Sign\-On** and then choose **Enable access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Single Sign\-On that they can now enable that service to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Single Sign\-On as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle sso.amazonaws.com
  ```

  The previous command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS SSO<a name="integrate-disable-ta-sso"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

AWS SSO requires trusted access with AWS Organizations to operate\. If you disable trusted access using AWS Organizations while you are using AWS SSO, it stops functioning because it can't access the organization\. Users can't use AWS SSO to access accounts\. Any roles that AWS SSO creates remain, but the AWS SSO service can't access them\. The AWS SSO service\-linked roles remain\. If you reenable trusted access, AWS SSO continues to operate as before, without the need for you to reconfigure the service\. 

If you remove an account from your organization, AWS SSO automatically cleans up any metadata and resources, such as its service\-linked role\. A standalone account that is removed from an organization no longer works with AWS SSO\.