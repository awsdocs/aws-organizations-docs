# Service quotas and AWS Organizations<a name="services-that-can-integrate-service-quotas"></a>

Service Quotas is an AWS service that enables you to view and manage your quotas from a central location\. Quotas, also referred to as limits, are the maximum value for your resources, actions, and items in your AWS account\. When Service Quotas is associated with AWS Organizations, you can create a quota request template to automatically request quota increases when accounts are created\. For more information about Service Quotas, see the [https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html)\.

The following list provides information that is useful to know when you want to associate Service Quotas and AWS Organizations: 
+ **To enable trusted access with AWS Organizations:** Sign in with your AWS Organizations master account and then configure the template on the Service Quotas console\. For more information, see [Using the Service Quota Template](https://docs.aws.amazon.com/servicequotas/latest/userguide/organization-templates.html) in the *Service Quotas User Guide*\. 

  You can also call the [AssociateServiceQuotaTemplate](https://docs.aws.amazon.com/servicequotas/2019-06-24/apireference/API_AssociateServiceQuotaTemplate.html) operation\. For more information, see the [*Service Quotas API Reference*\.](https://docs.aws.amazon.com/servicequotas/2019-06-24/apireference/Welcome.html) 
+ **To disable trusted access with AWS Organizations:** Call the [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html) operation\.
+ **Service principal name for Service Quotas: ** `servicequotas.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts when trusted access is enabled: ** `AWSServiceRoleForServiceQuotas`