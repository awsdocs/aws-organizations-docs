# AWS Service Catalog and AWS Organizations<a name="services-that-can-integrate-servicecatalog"></a>

AWS Service Catalog enables you to create and manage catalogs of IT services that are approved for use on AWS\.

The integration of AWS Service Catalog with AWS Organizations simplifies the sharing of portfolios and copying of products across an organization\. AWS Service Catalog administrators can reference an existing organization in AWS Organizations when sharing a portfolio, and they can share the portfolio with any trusted organizational unit \(OU\) in the organization's tree structure\. This eliminates the need to share portfolio IDs, and for the receiving account to manually reference the portfolio ID when importing the portfolio\. Portfolios shared via this mechanism are listed in the shared\-to account in the administrator’s **Imported Portfolio** view in AWS Service Catalog\.

For more information about AWS Service Catalog, see the [https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html)\.

Use the following information to help you to help you integrate AWS Service Catalog with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-servicecatalog"></a>

AWS Service Catalog doesn't create any service\-linked roles as part of enabling trusted access\.

## Service principals used to grant permissions<a name="integrate-enable-svcprin-servicecatalog"></a>

To enable trusted access, you must specify the following service principal:
+ `servicecatalog.amazonaws.com`

## Enabling trusted access with AWS Service Catalog<a name="integrate-enable-ta-servicecatalog"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Service Catalog console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Service Catalog console or tools to enable integration with Organizations\. This lets AWS Service Catalog perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Service Catalog\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Service Catalog console or tools then you don’t need to complete these steps\.

**To enable trusted access using the AWS Service Catalog CLI or AWS SDK**  
Call one of the following commands or operations:
+ AWS CLI: [aws servicecatalog enable\-aws\-organizations\-access](https://docs.aws.amazon.com/cli/latest/reference/servicecatalog/enable-aws-organizations-access.html)
+ AWS SDKs: [AWSServiceCatalog::EnableAWSOrganizationsAccess](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_EnableAWSOrganizationsAccess.html)

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Service Catalog**, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, choose **Enable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Service Catalog that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ New console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Service Catalog**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Service Catalog that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Service Catalog as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal servicecatalog.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Service Catalog<a name="integrate-disable-ta-servicecatalog"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

If you disable trusted access using AWS Organizations while you are using AWS Service Catalog, it doesn't delete your current shares, but it prevents you from creating new shares throughout your organization\. Current shares won't be in sync with your organization structure if it changes after you call this action\.

You can disable trusted access using either the AWS Service Catalog console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Service Catalog console or tools to disable integration with Organizations\. This lets AWS Service Catalog perform any clean up that it requires, such as deleting resources or access roles that are no longer needed by the service\. Proceed with these steps only if you can’t disable integration using the tools provided by AWS Service Catalog\.  
If you disable trusted access by using the AWS Service Catalog console or tools then you don’t need to complete these steps\.

**To disable trusted access using the AWS Service Catalog CLI or AWS SDK**  
Call one of the following commands or operations:
+ AWS CLI: [aws servicecatalog enable\-aws\-organizations\-access](https://docs.aws.amazon.com/cli/latest/reference/servicecatalog/enable-aws-organizations-access.html)
+ AWS SDKs: [DisableAWSOrganizationsAccess](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_DisableAWSOrganizationsAccess.html)

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Service Catalog** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Service Catalog that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Service Catalog** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Service Catalog that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Service Catalog as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal servicecatalog.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------