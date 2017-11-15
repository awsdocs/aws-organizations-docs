# Integrating AWS Organizations with Other AWS Services<a name="orgs_integrate_services"></a>

AWS Organizations not only enables you to manage the accounts in your organization, but it also enables select AWS services to perform tasks on your behalf in your organization's member accounts\. This helps ensure that all accounts are consistent across your organization\. 

To achieve this integration between Organizations and another service, Organizations uses an IAM role called a *service\-linked role*\. 

## AWS Organizations and Service\-Linked Roles<a name="orgs_integrate_services-using_slrs"></a>

AWS Organizations uses [IAM service\-linked roles](http://aws.amazon.com/blogs/security/introducing-an-easier-way-to-delegate-permissions-to-aws-services-service-linked-roles/) to enable other AWS services to perform actions on your behalf in your organization's member accounts\. When you configure another service and authorize it to integrate with your organization, Organizations creates a service\-linked role in each member account\. The service\-linked role has predefined IAM permissions that allow the other service to perform specific tasks within your organization's accounts\. In general, AWS manages all service\-linked roles, which means that you typically can't alter the roles or the attached policies\. 

To make all of this possible, when you create an account in an organization or you accept an invitation to join your existing account to an organization, Organizations automatically provisions the account with a service\-linked role named *AWSServiceRoleForOrganizations*\. This role can be assumed only by the Organizations service itself\. The role has permissions that only allows Organizations to create service\-linked roles for other AWS services\. 

Although we don't recommend it, if your organization has only consolidated billing features enabled, the service\-linked role is never used and you can delete the *AWSServiceRoleForOrganizations* service\-linked role\. If you later want to enable all features in your organization, the role is required and must be restored\. The following checks occur when you begin the process to enable all features:

+ **For each member account that was *invited to join* the organization** – The account administrator receives a request agreeing to enable all features\. If a service\-linked role doesn't already exist, Organizations creates it automatically when the administrator agrees to the request\. The administrator must have both `organizations:AcceptHandshake` **and** `iam:CreateServiceLinkedRole` permissions to successfully agree to the request\. If the *AWSServiceRoleForOrganizations* role already exists, the administrator needs only the `organizations:AcceptHandshake` permission to agree to the request\.

+ **For each member account that was *created* in the organization** – The account administrator receives a request to re\-create the service\-linked role\. \(The administrator of the member account doesn't receive a request to enable all features because the administrator of the master account is considered the owner of the created member accounts\.\) Organizations creates the service\-linked role automatically when the member account administrator agrees to the request\. The administrator must have both `organizations:AcceptHandshake` **and** `iam:CreateServiceLinkedRole` permissions to successfully accept the handshake\.

After you enable all features in your organization, you no longer can delete the *AWSServiceRoleForOrganizations* service\-linked role from any account\.

**Important**  
Service\-linked roles are never affected by Organizations service control policies \(SCPs\)\. They are automatically exempt from any SCP restrictions\.