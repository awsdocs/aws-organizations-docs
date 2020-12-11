# Service Quotas and AWS Organizations<a name="services-that-can-integrate-servicequotas"></a>

Service Quotas is an AWS service that enables you to view and manage your quotas from a central location\. Quotas, also referred to as limits, are the maximum value for your resources, actions, and items in your AWS account\.

When Service Quotas is associated with AWS Organizations, you can create a quota request template to automatically request quota increases when accounts are created\.

For more information about Service Quotas, see the [Service Quotas User Guide](https://docs.aws.amazon.com/servicequotas/latest/userguide/)\.

Use the following information to help you to help you integrate Service Quotas with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-servicequotas)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-servicequotas)
+ [Enabling trusted access with Service Quotas](#integrate-enable-ta-servicequotas)
+ [Disabling trusted access with Service Quotas](#integrate-disable-ta-servicequotas)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-servicequotas"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow Service Quotas to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Service Quotas and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForServiceQuotas`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-servicequotas"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Service Quotas grant access to the following service principals:
+ `servicequotas.amazonaws.com`

## Enabling trusted access with Service Quotas<a name="integrate-enable-ta-servicequotas"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the Service Quotas console or the AWS Organizations console\.

**To enable trusted access using the Service Quotas console**  
Sign in with your AWS Organizations management account and then configure the template on the Service Quotas console\. For more information, see [Using the Service Quota Template](https://docs.aws.amazon.com/servicequotas/latest/userguide/organization-templates.html) in the *Service Quotas User Guide*\. 

**To enable trusted access using the Service Quotas AWS CLI or SDK**  
Call the following command or operation:
+ AWS CLI: [aws service\-quotas associate\-service\-quota\-template](https://docs.aws.amazon.com/cli/latest/reference/service-quotas/API_AssociateServiceQuotaTemplate.html)
+ AWS SDKs: [AssociateServiceQuotaTemplate](https://docs.aws.amazon.com/servicequotas/2019-06-24/apireference/API_AssociateServiceQuotaTemplate.html)

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the Service Quotas console or tools to enable integration with Organizations so that Service Quotas can perform any configuration that it requires\. Proceed with these steps only if you can’t enable integration using the tools provided by Service Quotas\.  
If you enable trusted access by using the tools provided by Service Quotas then you don’t need to complete these steps\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **Service Quotas**, choose the service’s name, and then choose Enable trusted access\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Service Quotas that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable Service Quotas as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle servicequotas.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with Service Quotas<a name="integrate-disable-ta-servicequotas"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using the AWS Organizations console, or by using the CLI or AWS SDK\.

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the Service Quotas console or tools to disable integration with Organizations so that Service Quotas can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not Service Quotas, wait until the administrator of Service Quotas tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **Service Quotas** and then choose the service’s name\.

1. In the confirmation dialog box, check the box for **Show the option to disable trusted access**, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Service Quotas that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable Service Quotas as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle servicequotas.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------