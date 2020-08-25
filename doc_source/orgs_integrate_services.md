# Enabling trusted access with other AWS services<a name="orgs_integrate_services"></a>

You can use *trusted access* to enable an AWS service that you specify, called the *trusted service*, to perform tasks in your organization and its accounts on your behalf\. This involves granting permissions to the trusted service but does *not* otherwise affect the permissions for IAM users or roles\. When you enable access, the trusted service can create an IAM role called a *service\-linked role* in every account in your organization whenever that role is needed\. That role has a permissions policy that allows the trusted service to do the tasks that are described in that service's documentation\. This enables you to specify settings and configuration details that you would like the trusted service to maintain in your organization's accounts on your behalf\. The trusted serviced only creates service\-linked roles when it needs to perform management actions on accounts, and not necessarily in all accounts of the organization\. 

**Important**  
We recommend that you enable trusted access by using the trusted service's console, the AWS CLI, or the API operations in one of the [AWS SDKs](https://aws.amazon.com/tools/)\. This enables the trusted service to perform any required initialization or create any required resources\. To learn what configuration the trusted service performs, see the documentation for that service\. 

## Permissions required to enable trusted access<a name="orgs_trusted_access_perms"></a>

Trusted access requires permissions for two services: AWS Organizations and the trusted service\. To enable trusted access, choose one of the following scenarios:
+ If you have credentials with permissions in both AWS Organizations and the trusted service, enable access by using the tools \(console or AWS CLI\) that are available in the trusted service\. This allows the trusted service to enable trusted access in AWS Organizations on your behalf and to create any resources required for the service to operate in your organization\.

  The minimum permissions for these credentials are the following:
  + `organizations:EnableAWSServiceAccess`\. You can also use the `organizations:ServicePrincipal` condition key with this operation to limit requests that those operations make to a list of approved service principal names\. For more information, see [Condition keys](orgs_permissions_overview.md#orgs_permissions_conditionkeys)\.
  + `organizations:ListAWSServiceAccessForOrganization` – Required if you use the AWS Organizations console\.
  + The minimum permissions that are required by the trusted service depend on the service\. For more information, see the trusted service's documentation\.
+ If one person has credentials with permissions in AWS Organizations but someone else has credentials with permissions in the trusted service, perform these steps in the following order:

  1. The person who has credentials with permissions in AWS Organizations should use the AWS Organizations console, the AWS CLI, or an AWS SDK to enable trusted access for the trusted service\. This grants permission to the other service to perform its required configuration in the organization when the following step \(step 2\) is performed\.

     The minimum AWS Organizations permissions are the following:
     + `organizations:EnableAWSServiceAccess`
     + `organizations:ListAWSServiceAccessForOrganization` – Required only if you use the AWS Organizations console

     For the steps to enable trusted access in AWS Organizations, see [How to enable or disable trusted access](#orgs_how-to-enable-disable-trusted-access)\.

  1. The person who has credentials with permissions in the trusted service enables that service to work with AWS Organizations\. This instructs the service to perform any required initialization, such as creating any resources that are required for the trusted service to operate in the organization\. For more information, see the service\-specific instructions at [Services that support trusted access with your organization](services-that-can-integrate.md)\.

## Permissions required to disable trusted access<a name="orgs_trusted_access_disable_perms"></a>

When you no longer want to allow the trusted service to operate on your organization or its accounts, choose one of the following scenarios\.

**Important**  
Disabling trusted service access does ***not*** prevent users and roles with appropriate permissions from using that service\. To completely block users and roles from accessing an AWS service, you can remove the IAM permissions that grant that access, or you can use [service control policies \(SCPs\)](orgs_manage_policies_scps.md) in AWS Organizations\.
+ If you have credentials with permissions in both AWS Organizations and the trusted service, disable access by using the tools \(console or AWS CLI\) that are available for the trusted service\. The service then cleans up by removing resources that are no longer required and by disabling trusted access for the service in AWS Organizations on your behalf\. 

  The minimum permissions for these credentials are the following:
  + `organizations:DisableAWSServiceAccess`\. You can also use the `organizations:ServicePrincipal` condition key with this operation to limit requests that those operations make to a list of approved service principal names\. For more information, see [Condition keys](orgs_permissions_overview.md#orgs_permissions_conditionkeys)\.
  + `organizations:ListAWSServiceAccessForOrganization` – Required if you use the AWS Organizations console\.
  + The minimum permissions required by the trusted service depend on the service\. For more information, see the trusted service's documentation\.
+ If the credentials with permissions in AWS Organizations aren't the credentials with permissions in the trusted service, perform these steps in the following order:

  1. The person with permissions in the trusted service first disables access using that service\. This instructs the trusted service to clean up by removing the resources required for trusted access\. For more information, see the service\-specific instructions at [Services that support trusted access with your organization](services-that-can-integrate.md)\.

  1. The person with permissions in AWS Organizations can then use the AWS Organizations console, AWS CLI, or an AWS SDK to disable access for the trusted service\. This removes the permissions for the trusted service from the organization and its accounts\. 

     The minimum AWS Organizations permissions are the following:
     + `organizations:DisableAWSServiceAccess`
     + `organizations:ListAWSServiceAccessForOrganization` – Required only if you use the AWS Organizations console

     For the steps to disable trusted access in AWS Organizations, see [How to enable or disable trusted access](#orgs_how-to-enable-disable-trusted-access)\.

## How to enable or disable trusted access<a name="orgs_how-to-enable-disable-trusted-access"></a>

If you have permissions only for AWS Organizations and you want to enable or disable trusted access to your organization on behalf of the administrator of the other AWS service, use the following procedure\.

**To enable or disable trusted service access \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. In the upper\-right corner, choose **Settings**\.

1. If you are *enabling* access, proceed to the next step\. If you are *disabling* access, wait until the administrator tells you that the service is disabled and the resources have been cleaned up\.

1. In the **Trusted access for AWS services** section, find the service that you want and then choose either **Enable access** or **Disable access** as appropriate\.

1. If you are *enabling* access, tell the administrator of the other AWS service that they can now enable the other service to work with AWS Organizations\.

**To enable or disable trusted service access \(AWS CLI, AWS API\)**

You can use the following AWS CLI commands or API operations to enable or disable trusted service access:
+ AWS CLI: aws organizations [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)
+ AWS CLI: aws organizations [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

## AWS Organizations and service\-linked roles<a name="orgs_integrate_services-using_slrs"></a>

AWS Organizations uses [IAM service\-linked roles](http://aws.amazon.com/blogs/security/introducing-an-easier-way-to-delegate-permissions-to-aws-services-service-linked-roles/) to enable trusted services to perform tasks on your behalf in your organization's member accounts\. When you configure a trusted service and authorize it to integrate with your organization, that service can request that AWS Organizations create a service\-linked role in its member account\. The trusted service does this asynchronously as needed and not necessarily in all accounts in the organization at the same time\. The service\-linked role has predefined IAM permissions that allow the trusted service to perform specific tasks within that account\. In general, AWS manages all service\-linked roles, which means that you typically can't alter the roles or the attached policies\. 

To make all of this possible, when you create an account in an organization or you accept an invitation to join your existing account to an organization, AWS Organizations provisions the account with a service\-linked role named `AWSServiceRoleForOrganizations`\. Only the AWS Organizations service itself can assume this role\. The role has permissions that allow only AWS Organizations to create service\-linked roles for other AWS services\. This service\-linked role is present in all organizations\.

Although we don't recommend it, if your organization has only [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) enabled, the service\-linked role named `AWSServiceRoleForOrganizations` is never used, and you can delete it\. If you later want to enable [all features](orgs_getting-started_concepts.md#feature-set-all) in your organization, the role is required, and you must restore it\. The following checks occur when you begin the process to enable all features:
+ **For each member account that was *invited to join* the organization** – The account administrator receives a request to agree to enable all features\. To successfully agree to the request, the administrator must have both `organizations:AcceptHandshake` *and* `iam:CreateServiceLinkedRole` permissions if the service\-linked role \(`AWSServiceRoleForOrganizations`\) doesn't already exist\. If the `AWSServiceRoleForOrganizations` role already exists, the administrator needs only the `organizations:AcceptHandshake` permission to agree to the request\. When the administrator agrees to the request, AWS Organizations creates the service\-linked role if it doesn't already exist\. 
+ **For each member account that was *created* in the organization** – The account administrator receives a request to recreate the service\-linked role\. \(The administrator of the member account doesn't receive a request to enable all features because the administrator of the master account is considered the owner of the created member accounts\.\) AWS Organizations creates the service\-linked role when the member account administrator agrees to the request\. The administrator must have both `organizations:AcceptHandshake` *and* `iam:CreateServiceLinkedRole` permissions to successfully accept the handshake\.

After you enable all features in your organization, you no longer can delete the `AWSServiceRoleForOrganizations` service\-linked role from any account\.

**Important**  
AWS Organizations SCPs never affect service\-linked roles\. These roles are exempt from any SCP restrictions\.