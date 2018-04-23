# Enabling Trusted Access with Other AWS Services<a name="orgs_integrate_services"></a>

You can use *trusted access* to enable an AWS service that you specify, called the *trusted service*, to perform tasks in your organization and its accounts on your behalf\. It involves granting permissions to the trusted service but does *not* otherwise affect the permissions for IAM users or roles\. When you enable access, the trusted service can create an IAM role called a *service\-linked role* in every account in your organization\. That role has a permissions policy that enables the trusted service to do the tasks that are described in that service's documentation\. This allows you to specify settings and configuration details that you would like the trusted service to maintain in your organization's accounts on your behalf\. The roles are created asynchronously as needed by the trusted service, and not necessarily in all accounts of the organization\.

**Important**  
We recommend that you enable trusted access by using the trusted service's console, the AWS CLI, or the API operations in one of the AWS SDKs\. This enables the trusted service to perform any required initialization or create any required resources\. To learn what configuration the trusted service performs, see the documentation for that service\. 

## Permissions Required to Enable Trusted Access<a name="orgs_trusted_access_perms"></a>

Trusted access requires permissions for two services: AWS Organizations and the trusted service\. To enable trusted access, choose one of the following scenarios:
+ If you have credentials that have permissions in both AWS Organizations and the trusted service, enable access by using the tools \(console or CLI\) that are available in the trusted service\. This allows the trusted service to enable trusted access in Organizations on your behalf and to create any resources required for the service to operate in your organization\.

  The minimum permissions for these credentials are the following:
  + `organizations:EnableAWSServiceAccess`
  + `organizations:ListAWSServiceAccessForOrganization` — Required if you use the Organizations console
  + The minimum permissions required by the trusted service depend on the service\. For more information, see the trusted service's documentation\.
+ If one person has credentials with permissions in Organizations but someone else has credentials with permissions in the trusted service, perform these steps in the following order:

  1. The person who has credentials with permissions in Organizations should use the Organizations console, the AWS CLI, or an AWS SDK to enable trusted access for the trusted service\. This grants permission to the other service to perform its required configuration in the organization when the following step \(step 2\) is performed\.

     The minimum Organizations permissions are the following:
     + `organizations:EnableAWSServiceAccess`
     + `organizations:ListAWSServiceAccessForOrganization` — required only if you use the Organizations console

     For the specific steps to enable trusted access in Organizations, see [How to Enable or Disable Trusted Access](#orgs_how-to-enable-disable-trusted-access)\.

  1. The person who has credentials with permissions in the trusted service can then enable that service to work with AWS Organizations\. This instructs the service to perform any required initialization, such as creating any resources that are required for the trusted service to operate in the organization\. For more information, see the service\-specific instructions at [Services That Support Trusted Access with Your Organization](#services-that-can-integrate)\.

## Permissions Required to Disable Trusted Access<a name="orgs_trusted_access_disable_perms"></a>

When you no longer want to allow the trusted service to operate on your organization or its accounts, choose one of the following scenarios:

**Important**  
Disabling trusted service access does ***not*** prevent users and roles with appropriate permissions from using that service\. To completely block users and roles from accessing an AWS service, you can remove the IAM permissions that grant that access, or you can use [service control policies \(SCPs\)](orgs_manage_policies_scp.md) in AWS Organizations\.
+ If you have credentials that have permissions in both Organizations and the trusted service, disable access by using the tools \(console or CLI\) that are available for the trusted service\. The service then cleans up by removing resources that are no longer required and by disabling trusted access for the service in Organizations on your behalf\. 

  The minimum permissions for these credentials are the following:
  + `organizations:DisableAWSServiceAccess`
  + `organizations:ListAWSServiceAccessForOrganization` — Required if you use the Organizations console
  + The minimum permissions required by the trusted service depend on the service\. For more information, see the trusted service's documentation\.
+ If the credentials with permissions in Organizations are not the credentials with permissions in the trusted service, perform these steps in the following order:

  1. The person with permissions in the trusted service first disables access using that service\. This instructs the trusted service to clean up by removing the resources required for trusted access\. For more information, see the service\-specific instructions at [Services That Support Trusted Access with Your Organization](#services-that-can-integrate)\.

  1. The person with permissions in Organizations can then use the Organizations console, AWS CLI, or an AWS SDK to disable access for the trusted service\. This removes the permissions for the trusted service from your organization and its accounts\. 

     The minimum Organizations permissions are the following:
     + `organizations:DisableAWSServiceAccess`
     + `organizations:ListAWSServiceAccessForOrganization` — required only if you use the Organizations console

     For the steps to perform this, see [How to Enable or Disable Trusted Access](#orgs_how-to-enable-disable-trusted-access)\.

## How to Enable or Disable Trusted Access<a name="orgs_how-to-enable-disable-trusted-access"></a>

If you have permissions only for Organizations, and you want to enable or disable trusted access to your organization on behalf of the administrator of the other AWS service, use the following procedure\.

**To enable or disable trusted service access \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. In the upper\-right corner, choose **Settings**\.

1. If you are *enabling* access, you can proceed to the next step\. If you are *disabling* access, wait until the administrator tells you that the service is disabled and the resources have been cleaned up\.

1. In the **Trusted access for AWS services** section, find the service that you want and then choose either **Enable access** or **Disable access**, as appropriate\.

1. If you are *enabling* access, tell the administrator of the other AWS service that they can now enable the other service to work with AWS Organizations\.

**To enable or disable trusted service access \(AWS CLI, AWS API\)**

You can use the following commands to enable or disable trusted service access:
+ AWS CLI: aws organizations [enable\-aws\-service\-access](http://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)
+ AWS CLI: aws organizations [disable\-aws\-service\-access](http://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)
+ AWS API: [EnableAWSServiceAccess](http://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)
+ AWS API: [DisableAWSServiceAccess](http://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

## AWS Organizations and Service\-Linked Roles<a name="orgs_integrate_services-using_slrs"></a>

AWS Organizations uses [IAM service\-linked roles](http://aws.amazon.com/blogs/security/introducing-an-easier-way-to-delegate-permissions-to-aws-services-service-linked-roles/) to enable trusted services to perform tasks on your behalf in your organization's member accounts\. When you configure a trusted service and authorize it to integrate with your organization, that service can request that Organizations create a service\-linked role in its member account\. This happens asynchronously as needed by the trusted service and not necessarily in all accounts in the organization at the same time\. The service\-linked role has predefined IAM permissions that allow the trusted service to perform specific tasks within that account\. In general, AWS manages all service\-linked roles, which means that you typically can't alter the roles or the attached policies\. 

To make all of this possible, when you create an account in an organization or you accept an invitation to join your existing account to an organization, Organizations provisions the account with a service\-linked role named *AWSServiceRoleForOrganizations*\. This role can be assumed only by the Organizations service itself\. The role has permissions that allows only Organizations to create service\-linked roles for other AWS services\. This service\-linked role is present in all organizations\.

Although we don't recommend it, if your organization has only [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) enabled, the service\-linked role named *AWSServiceRoleForOrganizations* is never used and you can delete it\. If you later want to enable [all features](orgs_getting-started_concepts.md#feature-set-all) in your organization, the role is required and you must restore it\. The following checks occur when you begin the process to enable all features:
+ **For each member account that was *invited to join* the organization** – The account administrator receives a request to agree to enable all features\. To successfully agree to the request, the administrator must have both `organizations:AcceptHandshake` **and** `iam:CreateServiceLinkedRole` permissions if the service\-linked role \(*AWSServiceRoleForOrganizations*\) doesn't already exist\. If the *AWSServiceRoleForOrganizations* role already exists, the administrator needs only the `organizations:AcceptHandshake` permission to agree to the request\. When the administrator agrees to the request, Organizations creates the service\-linked role if it doesn't already exist\. 
+ **For each member account that was *created* in the organization** – The account administrator receives a request to recreate the service\-linked role\. \(The administrator of the member account doesn't receive a request to enable all features because the administrator of the master account is considered the owner of the created member accounts\.\) Organizations creates the service\-linked role when the member account administrator agrees to the request\. The administrator must have both `organizations:AcceptHandshake` **and** `iam:CreateServiceLinkedRole` permissions to successfully accept the handshake\.

After you enable all features in your organization, you no longer can delete the *AWSServiceRoleForOrganizations* service\-linked role from any account\.

**Important**  
Service\-linked roles are never affected by Organizations service control policies \(SCPs\)\. They are exempt from any SCP restrictions\.

## Services That Support Trusted Access with Your Organization<a name="services-that-can-integrate"></a>

The following sections describe the AWS services for which you can enable trusted access with your organization\. Each section includes the following:
+ A summary of the trusted service and how it works when you enable trusted access\.
+ Links to instructions for enabling and disabling trusted access with your organization\.
+ The principal name of the trusted service that you can specify in policies to grant the trusted access to the accounts in your organization\. 
+ If applicable, the name of the IAM service\-linked role created in all accounts when you enable trusted access\.

### AWS Config<a name="services-that-can-integrate-config"></a>

Multi\-account multi\-region data aggregation in AWS Config allows you to aggregate AWS Config data from multiple accounts and regions into a single account\. Multi\-account multi\-region data aggregation is useful for central IT administrators to monitor compliance for multiple AWS accounts in the enterprise\. An aggregator is a new resource type in AWS Config that collects AWS Config data from multiple source accounts and regions\. Create an aggregator in the region where you want to see the aggregated AWS Config data\. While creating an aggregator, you can either select adding individual account IDs or adding your organization\. For more information about AWS Config, see the [AWS Config Developer Guide](http://docs.aws.amazon.com/config/latest/developerguide/)\.

The following list provides information that is useful to know when you want to integrate AWS Config and Organizations:
+ **To enable trusted access with Organizations:** To enable trusted access to Organizations from AWS Config, you create a multi\-account aggregator and add the organization\. For information on how to configure a multi\-account aggregator, see [Setting Up an Aggregator Using the Console](http://docs.aws.amazon.com/config/latest/developerguide/setup-aggregator-console.html) in the *AWS Config Developer Guide*\.
+ **Service principal name for AWS Config** `config.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts** when trusted access is enabled: AWSConfigRoleForOrganizations

### AWS Firewall Manager<a name="services-that-can-integrate-fms"></a>

AWS Firewall Manager is a security management service to centrally configure and manage web application firewall rules across your accounts and applications\. Using Firewall manager, you can roll out AWS WAF rules all at once for your Application Load Balancers and Amazon CloudFront distributions across all of the accounts in your AWS organization\. Use Firewall Manager to set up your firewall rules just once and have them automatically applied across all accounts and resources within your organization, even as new resources and accounts are added\. For more information about Firewall Manager, see the [AWS Firewall Developer Guide](http://docs.aws.amazon.com/waf/latest/developerguide/%5E-fms-chapter.html)\.

The following list provides information that is useful to know when you want to integrate Firewall Manager and Organizations:
+ **To enable trusted access with Organizations:** You must sign in with your AWS Organizations master account to configure an account within the organization as the AWS Firewall Manager administrator account\. For information, see [Step 2: Assign a master account](http://docs.aws.amazon.com/waf/latest/developerguide/enable-integration.html) in the *AWS Firewall Manager Developer Guide*
+ **To disable trusted access with Organizations:** You can change or revoke the AWS Firewall administrator account by following the instructions in [Setting a new AWS Firewall Manager administrator account](http://docs.aws.amazon.com/waf/latest/developerguide/fms-change-administrator.html)\. If you revoke the administrator account, you must sign in to the Organizations master account and set a new administrator account for AWS Firewall Manager\.
+ **Service principal name for AWS FMS:** `fms.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts** when trusted access is enabled: `AWSServiceRoleForFMS`

### AWS Single Sign\-On<a name="services-that-can-integrate-peregrine"></a>

AWS Single Sign\-On \(AWS SSO\) provides single sign\-on services for all of your AWS accounts and cloud applications\. It connects with Microsoft Active Directory through AWS Directory Service to allow users in that directory to sign in to a personalized user portal using their existing Active Directory user names and passwords\. From the portal, users have access to all the AWS accounts and cloud applications that you provide in the portal\. For more information about AWS SSO, see the [AWS Single Sign\-On User Guide](http://docs.aws.amazon.com/singlesignon/latest/userguide/)\.

The following list provides information that is useful to know when you want to integrate AWS SSO and Organizations:
+ **To enable trusted access with Organizations:** AWS SSO requires trusted access with AWS Organizations to function\. Trusted access is enabled when you set up AWS SSO\. For more information, see [Getting Started \- Step 1: Enable AWS Single Sign\-On](http://docs.aws.amazon.com/singlesignon/latest/userguide/step1.html) in the *AWS Single Sign\-On User Guide*\.
+ **To disable trusted access with Organizations:** AWS SSO requires trusted access with AWS Organizations to operate\. If you disable trusted access using Organizations while you are using AWS SSO, it stops functioning because it cannot access the organization\. Users cannot use SSO to access accounts\. Any roles that AWS SSO creates remain, but can't be accessed by the AWS SSO service\. The AWS SSO service\-linked roles remain\. If you then re\-enable trusted access, AWS SSO continues to operate as before, without the need for you to reconfigure the service\. 

  If you remove an account from your organization, AWS SSO automatically cleans up any metadata and resources, such as its service\-linked role\. A standalone account that is removed from an organization no longer works with AWS SSO\.
+ **Service principal name for AWS SSO:** `sso.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts** when trusted access is enabled: `AWSServiceRoleForSSO`

  For more information, see [Using Service\-Linked Roles for AWS SSO](http://docs.aws.amazon.com/singlesignon/latest/userguide/using-service-linked-roles.html) in the *AWS Single Sign\-On User Guide*\.