# AWS Firewall Manager and AWS Organizations<a name="services-that-can-integrate-fms"></a>

AWS Firewall Manager is a security management service that centrally configures and manages firewall rules for web applications across your accounts and applications\. Using AWS Firewall Manager, you can roll out AWS WAF rules all at once for your Application Load Balancers and Amazon CloudFront distributions across all of the accounts in your AWS organization\. Use AWS Firewall Manager to set up your firewall rules just once and have them automatically applied across all accounts and resources within your organization, even as new resources and accounts are added\. For more information about AWS Firewall Manager, see the [AWS Firewall Manager Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/fms-chapter.html)\.

Use the following information to help you to help you integrate AWS Firewall Manager with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-fms)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-fms)
+ [Enabling trusted access with Firewall Manager](#integrate-enable-ta-fms)
+ [Disabling trusted access with Firewall Manager](#integrate-disable-ta-fms)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-fms"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow Firewall Manager to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Firewall Manager and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForFMS`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-fms"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Firewall Manager grant access to the following service principals:
+ `fms.amazonaws.com`

## Enabling trusted access with Firewall Manager<a name="integrate-enable-ta-fms"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Firewall Manager console or the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by using the Firewall Manager console\. This enables Firewall Manager to perform required setup tasks\.

You must sign in with your AWS Organizations management account and configure an account within the organization as the AWS Firewall Manager administrator account\. For more information, see [Step 2: Set the AWS Firewall Manager Administrator Account](https://docs.aws.amazon.com/waf/latest/developerguide/enable-integration.html) in the *AWS Firewall Manager Developer Guide*\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find the row for **AWS Firewall Manager** and then choose **Enable access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Firewall Manager that they can now enable that service to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Firewall Manager as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal fms.amazonaws.com
  ```

  This command produces no output if successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with Firewall Manager<a name="integrate-disable-ta-fms"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can change or revoke the AWS Firewall Manager administrator account by following the instructions in [Designating a Different Account as the AWS Firewall Manager Administrator Account](https://docs.aws.amazon.com/waf/latest/developerguide/fms-change-administrator.html) in the *AWS Firewall Manager Developer Guide*\.

If you revoke the administrator account, you must sign in to the AWS Organizations management account and set a new administrator account for AWS Firewall Manager\.

On the Organizations side, you can disable trusted access by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Firewall Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal fms.amazonaws.com
  ```

  This command produces no output if successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------