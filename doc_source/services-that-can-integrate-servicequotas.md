# Service Quotas and AWS Organizations<a name="services-that-can-integrate-servicequotas"></a>

Service Quotas is an AWS service that enables you to view and manage your quotas from a central location\. Quotas, also referred to as limits, are the maximum value for your resources, actions, and items in your AWS account\.

When Service Quotas is associated with AWS Organizations, you can create a quota request template to automatically request quota increases when accounts are created\.

For more information about Service Quotas, see the [Service Quotas User Guide](https://docs.aws.amazon.com/servicequotas/latest/userguide/)\.

Use the following information to help you to help you integrate Service Quotas with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-servicequotas)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-servicequotas)
+ [Enabling trusted access with Service Quotas](#integrate-enable-ta-servicequotas)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-servicequotas"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow Service Quotas to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Service Quotas and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForServiceQuotas`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-servicequotas"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Service Quotas grant access to the following service principals:
+ `servicequotas.amazonaws.com`

## Enabling trusted access with Service Quotas<a name="integrate-enable-ta-servicequotas"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using the Service Quotas console

**To enable trusted access using the Service Quotas console**  
Sign in with your AWS Organizations management account and then configure the template on the Service Quotas console\. For more information, see [Using the Service Quota Template](https://docs.aws.amazon.com/servicequotas/latest/userguide/organization-templates.html) in the *Service Quotas User Guide*\. 

**To enable trusted access using the Service Quotas AWS CLI or SDK**  
Call the following command or operation:
+ AWS CLI: [aws service\-quotas associate\-service\-quota\-template](https://docs.aws.amazon.com/cli/latest/reference/service-quotas/API_AssociateServiceQuotaTemplate.html)
+ AWS SDKs: [AssociateServiceQuotaTemplate](https://docs.aws.amazon.com/servicequotas/2019-06-24/apireference/API_AssociateServiceQuotaTemplate.html)