# AWS Service Catalog and AWS Organizations<a name="services-that-can-integrate-servicecatalog"></a>

AWS Service Catalog enables you to create and manage catalogs of IT services that are approved for use on AWS\.

The integration of AWS Service Catalog with AWS Organizations simplifies the sharing of portfolios and copying of products across an organization\. AWS Service Catalog administrators can reference an existing organization in AWS Organizations when sharing a portfolio, and they can share the portfolio with any trusted organizational unit \(OU\) in the organization's tree structure\. This eliminates the need to share portfolio IDs, and for the receiving account to manually reference the portfolio ID when importing the portfolio\. Portfolios shared via this mechanism are listed in the shared\-to account in the administrator’s **Imported Portfolio** view in AWS Service Catalog\.

For more information about AWS Service Catalog, see the [https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html)\.

Use the following information to help you to help you integrate AWS Service Catalog with AWS Organizations\.

**Topics**
+ [Service principals used to grant permissions](#integrate-enable-svcprin-servicecatalog)
+ [Enabling trusted access with AWS Service Catalog](#integrate-enable-ta-servicecatalog)
+ [Disabling trusted access with AWS Service Catalog](#integrate-disable-ta-servicecatalog)

## Service principals used to grant permissions<a name="integrate-enable-svcprin-servicecatalog"></a>

To enable trusted access, you must specify the following service principal:
+ `servicecatalog.amazonaws.com`

## Enabling trusted access with AWS Service Catalog<a name="integrate-enable-ta-servicecatalog"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using the AWS Organizations console or by using the CLI or AWS SDK\.

**To enable trusted access using the AWS Service Catalog CLI or AWS SDK**  
Call one of the following commands or operations:
+ AWS CLI: [aws servicecatalog enable\-aws\-organizations\-access](https://docs.aws.amazon.com/cli/latest/reference/servicecatalog/enable-aws-organizations-access.html)
+ AWS SDKs: [AWSServiceCatalog::EnableAWSOrganizationsAccess](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_EnableAWSOrganizationsAccess.html)

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS Service Catalog console or tools to enable integration with Organizations so that AWS Service Catalog can perform any configuration that it requires\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Service Catalog\.  
If you enable trusted access by using the tools provided by AWS Service Catalog then you don’t need to complete these steps\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **AWS Service Catalog**, choose the service’s name, and then choose Enable trusted access\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Service Catalog that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Service Catalog as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle servicecatalog.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Service Catalog<a name="integrate-disable-ta-servicecatalog"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

If you disable trusted access using AWS Organizations while you are using AWS Service Catalog, it doesn't delete your current shares, but it prevents you from creating new shares throughout your organization\. Current shares won't be in sync with your organization structure if it changes after you call this action\.

You can disable trusted access using the AWS Organizations console, or by using the CLI or AWS SDK\.

**To disable trusted access using the AWS Service Catalog CLI or AWS SDK**  
Call one of the following commands or operations:
+ AWS CLI: [aws servicecatalog enable\-aws\-organizations\-access](https://docs.aws.amazon.com/cli/latest/reference/servicecatalog/enable-aws-organizations-access.html)
+ AWS SDKs: [DisableAWSOrganizationsAccess](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_DisableAWSOrganizationsAccess.html)

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS Service Catalog console or tools to disable integration with Organizations so that AWS Service Catalog can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Service Catalog, wait until the administrator of AWS Service Catalog tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **AWS Service Catalog** and then choose the service’s name\.

1. In the confirmation dialog box, check the box for **Show the option to disable trusted access**, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Service Catalog that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Service Catalog as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle servicecatalog.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------