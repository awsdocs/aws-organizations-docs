# AWS Marketplace and AWS Organizations<a name="services-that-can-integrate-marketplace"></a>

AWS Marketplace is a curated digital catalog that you can use to find, buy, deploy, and manage third\-party software, data, and services that you need to build solutions and run your businesses\.

AWS Marketplace creates and manages licenses using AWS License Manager for your purchases in AWS Marketplace\. When you share \(grant access to\) your licenses with other accounts in your organization, AWS Marketplace creates and manages new licenses for those accounts\. 

For more information, see [Service\-linked roles for AWS Marketplace](https://docs.aws.amazon.com/marketplace/latest/buyerguide/buyer-using-service-linked-roles.html) in the *AWS Marketplace Buyer Guide*\.

Use the following information to help you to help you integrate AWS Marketplace with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-marketplace)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-marketplace)
+ [Enabling trusted access with AWS Marketplace](#integrate-enable-ta-marketplace)
+ [Disabling trusted access with AWS Marketplace](#integrate-disable-ta-marketplace)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-marketplace"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Marketplace to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Marketplace and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForMarketplaceLicenseManagement`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-marketplace"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS Marketplace grant access to the following service principals:
+ `license-management.marketplace.amazonaws.com`

## Enabling trusted access with AWS Marketplace<a name="integrate-enable-ta-marketplace"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using the AWS Marketplace console\.

**To enable trusted access using the AWS Marketplace console**  
see [Creating a service\-linked role for AWS Marketplace](https://docs.aws.amazon.com/marketplace/latest/buyerguide/buyer-using-service-linked-roles.html#buyer-creating-service-linked-role) in the *AWS Marketplace Buyer Guide*\.

## Disabling trusted access with AWS Marketplace<a name="integrate-disable-ta-marketplace"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

On the Organizations side, you can disable trusted access by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS Marketplace console or tools to disable integration with Organizations so that AWS Marketplace can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Marketplace, wait until the administrator of AWS Marketplace tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Marketplace as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle license-management.marketplace.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------