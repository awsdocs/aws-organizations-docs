# AWS Single Sign\-On and AWS Organizations<a name="services-that-can-integrate-sso"></a>

AWS Single Sign\-On \(AWS SSO\) provides single sign\-on services for all of your AWS accounts and cloud applications\. It connects with Microsoft Active Directory through AWS Directory Service to allow users in that directory to sign in to a personalized user portal using their existing Active Directory user names and passwords\. From the portal, users have access to all the AWS accounts and cloud applications that you provide in the portal\.

For more information about AWS SSO, see the [AWS Single Sign\-On User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/)\.

Use the following information to help you integrate AWS Single Sign\-On with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-sso"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows AWS SSO to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between AWS SSO and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForSSO`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-sso"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS SSO grant access to the following service principals:
+ `sso.amazonaws.com`

## Enabling trusted access with AWS SSO<a name="integrate-enable-ta-sso"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Single Sign\-On console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Single Sign\-On console or tools to enable integration with Organizations\. This lets AWS Single Sign\-On perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Single Sign\-On\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Single Sign\-On console or tools then you don’t need to complete these steps\.

AWS SSO requires trusted access with AWS Organizations to function\. Trusted access is enabled when you set up AWS SSO\. For more information, see [Getting Started \- Step 1: Enable AWS Single Sign\-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/step1.html) in the *AWS Single Sign\-On User Guide*\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Single Sign\-On**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Single Sign\-On that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Single Sign\-On as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal sso.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS SSO<a name="integrate-disable-ta-sso"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

AWS SSO requires trusted access with AWS Organizations to operate\. If you disable trusted access using AWS Organizations while you are using AWS SSO, it stops functioning because it can't access the organization\. Users can't use AWS SSO to access accounts\. Any roles that AWS SSO creates remain, but the AWS SSO service can't access them\. The AWS SSO service\-linked roles remain\. If you reenable trusted access, AWS SSO continues to operate as before, without the need for you to reconfigure the service\. 

If you remove an account from your organization, AWS SSO automatically cleans up any metadata and resources, such as its service\-linked role\. A standalone account that is removed from an organization no longer works with AWS SSO\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Single Sign\-On** and then choose the service’s name\.

1. Choose **Disable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Single Sign\-On that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Single Sign\-On as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal sso.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------