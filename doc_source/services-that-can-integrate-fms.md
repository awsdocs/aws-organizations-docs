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

You must enable trusted access using the AWS Firewall Manager console\.

You must sign in with your AWS Organizations management account and configure an account within the organization as the AWS Firewall Manager administrator account\. For more information, see [Step 2: Set the AWS Firewall Manager Administrator Account](https://docs.aws.amazon.com/waf/latest/developerguide/enable-integration.html) in the *AWS Firewall Manager Developer Guide*\.

## Disabling trusted access with Firewall Manager<a name="integrate-disable-ta-fms"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can change or revoke the AWS Firewall Manager administrator account by following the instructions in [Designating a Different Account as the AWS Firewall Manager Administrator Account](https://docs.aws.amazon.com/waf/latest/developerguide/fms-change-administrator.html) in the *AWS Firewall Manager Developer Guide*\.

If you revoke the administrator account, you must sign in to the AWS Organizations management account and set a new administrator account for AWS Firewall Manager\.

On the Organizations side, you can disable trusted access by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS Firewall Manager console or tools to disable integration with Organizations so that AWS Firewall Manager can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Firewall Manager, wait until the administrator of AWS Firewall Manager tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Firewall Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle fms.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------