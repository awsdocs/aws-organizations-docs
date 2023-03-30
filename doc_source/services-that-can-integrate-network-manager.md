# AWS Network Manager and AWS Organizations<a name="services-that-can-integrate-network-manager"></a>

Network Manager enables you to centrally manage your AWS Cloud WAN core network and your AWS Transit Gateway network across AWS accounts, Regions, and on\-premises locations\. With multi\-account support you can create a single global network for any of your AWS accounts, and register transit gateways from multiple accounts to the global network using the Network Manager console\. 

With trusted access between Network Manager and Organizations enabled, the registered delegated administrators and the management accounts can leverage the service\-linked role deployed in the member accounts to describe resources attached to your global networks\. From the Network Manager console the registered delegated administrators and the management accounts can assume the custom IAM roles deployed in the member accounts: `CloudWatch-CrossAccountSharingRole` for multi\-account monitoring and eventing, and `IAMRoleForAWSNetworkManagerCrossAccountResourceAccess` for the console switch role access for viewing and managing multi\-account resources\) 

**Important**  
We strongly recommend using the Network Manager console to manage multi\-account settings \(enable/disable trusted access and register/deregister delegated administrators\)\. Managing these settings from the console automatically deploys and manages all required service\-linked roles and custom IAM roles to the member accounts needed for multi\-account access\.
 When you enable trusted access for Network Manager in the Network Manager console, the console also enables AWS CloudFormation StackSets service\. Network Manager uses StackSets to deploy custom IAM roles needed for multi\-account management\.

For more information about integrating Network Manager with Organizations, see [Manage multiple accounts in Network Manager with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/tgwnm/tgw-nm-multi.html) in the *Amazon VPC User Guide*\.

Use the following information to help you integrate AWS Network Manager with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-network-manager"></a>

When you enable trusted access, the following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in the listed organization accounts\. These roles allow Network Manager to perform supported operations within the accounts in your organization\. If you disable trusted access, Network Manager will not delete these roles from accounts in your organization\. You can manually delete them using the IAM console\.

Management account
+  `AWSServiceRoleForNetworkManager`
+ ` AWSServiceRoleForCloudFormationStackSetsOrgAdmin`
+  `AWSServiceRoleForCloudWatchCrossAccount`

Member accounts
+  `AWSServiceRoleForNetworkManager`
+ ` AWSServiceRoleForCloudFormationStackSetsOrgMember`

When you register a member account as a delegated administrator, the following additional role is automatically created in the delegated administrator account:
+  `AWSServiceRoleForCloudWatchCrossAccount`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-network-manager"></a>

The service\-linked roles can only be assumed by the service principals authorized by the trust relationships defined for the role\. 
+ For the `AWSServiceRoleForNetworkManager service-linked` role, `networkmanager.amazonaws.com` is the only service principal that has access\. 
+ For the `AWSServiceRoleForCloudFormationStackSetsOrgMember` service\-linked role, `member.org.stacksets.cloudformation.amazonaws.com` is the only service principal that has access\. 
+ For the `AWSServiceRoleForCloudFormationStackSetsOrgAdmin` service\-linked role, `stacksets.cloudformation.amazonaws.com` is the only service principal that has access\. 
+ For the `AWSServiceRoleForCloudWatchCrossAccount` service\-linked role, `cloudwatch-crossaccount.amazonaws.com` is the only service principal that has access\. 

 Deleting these roles will impair multi\-account functionality for Network Manager\. 

## Enabling trusted access with Network Manager<a name="integrate-enable-ta-network-manager"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

Only an administrator in the Organizations management account has permissions to enable trusted access with another AWS service\. Be sure to use the Network Manager *console* to enable trusted access, to avoid permissions issues\. For more information, see [Manage multiple accounts in Network Manager with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/tgwnm/tgw-nm-multi.html) in the *Amazon VPC User Guide*\.

## Disabling trusted access with Network Manager<a name="integrate-disable-ta-network-manager"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in an Organizations management account has permissions to disable trusted access with another AWS service\. 

**Important**  
We strongly recommend using the Network Manager console to disable trusted access\. If you disable trusted access in any other way, such as using AWS CLI, with an API, or with the AWS CloudFormation console, deployed AWS CloudFormation StackSets and custom IAM roles may not be properly cleaned up\. To disable trusted service access, sign in to the [Network Manager console](https://console.aws.amazon.com/vpc/home#networkmanager)\. 

## Enabling a delegated administrator account for Network Manager<a name="integrate-enable-da-network-manager"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for Network Manager that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Network Manager\.

For instructions on how to designate a member account as a delegated administrator of Network Manager in the organization, see [Register a delegated administrator](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-delegated-admin.html) in the *Amazon VPC User Guide*\.
