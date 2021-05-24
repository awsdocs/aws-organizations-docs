# AWS Marketplace and AWS Organizations<a name="services-that-can-integrate-marketplace"></a>

AWS Marketplace is a curated digital catalog that you can use to find, buy, deploy, and manage third\-party software, data, and services that you need to build solutions and run your businesses\.

AWS Marketplace creates and manages licenses using AWS License Manager for your purchases in AWS Marketplace\. When you share \(grant access to\) your licenses with other accounts in your organization, AWS Marketplace creates and manages new licenses for those accounts\. 

For more information, see [Service\-linked roles for AWS Marketplace](https://docs.aws.amazon.com/marketplace/latest/buyerguide/buyer-using-service-linked-roles.html) in the *AWS Marketplace Buyer Guide*\.

Use the following information to help you integrate AWS Marketplace with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-marketplace"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Marketplace to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Marketplace and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForMarketplaceLicenseManagement`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-marketplace"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS Marketplace grant access to the following service principals:
+ `license-management.marketplace.amazonaws.com`

## Enabling trusted access with AWS Marketplace<a name="integrate-enable-ta-marketplace"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Marketplace console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Marketplace console or tools to enable integration with Organizations\. This lets AWS Marketplace perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Marketplace\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Marketplace console or tools then you don’t need to complete these steps\.

**To enable trusted access using the AWS Marketplace console**  
see [Creating a service\-linked role for AWS Marketplace](https://docs.aws.amazon.com/marketplace/latest/buyerguide/buyer-using-service-linked-roles.html#buyer-creating-service-linked-role) in the *AWS Marketplace Buyer Guide*\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Marketplace**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Marketplace that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Marketplace as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal license-management.marketplace.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Marketplace<a name="integrate-disable-ta-marketplace"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using only the Organizations tools\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Marketplace as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal license-management.marketplace.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------