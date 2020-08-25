# Service control policies<a name="orgs_manage_policies_scps"></a>

For information and procedures common to all policy types, see the following topics:
+ [Enable and disable policy types](orgs_manage_policies_enable-disable.md)
+ [Get details about your policies](orgs_manage_policies_info-operations.md)
+ [Policy syntax and inheritance](orgs_manage_policies_inheritance_auth.md)

## Service Control Policies \(SCPs\)<a name="orgs_manage_policies_scp_overview"></a>

Service control policies \(SCPs\) are a type of organization policy that you can use to manage permissions in your organization\. SCPs offer central control over the maximum available permissions for all accounts in your organization\. SCPs help you to ensure your accounts stay within your organization’s access control guidelines\. SCPs are available only in an organization that has [all features enabled](orgs_manage_org_support-all-features.md)\. SCPs aren't available if your organization has enabled only the consolidated billing features\. For instructions on enabling SCPs, see [Enabling and disabling policy types](orgs_manage_policies_enable-disable.md)\.

SCPs alone are not sufficient for allowing access in the accounts in your organization\. Attaching an SCP to an AWS Organizations entity \(root, organizational unit \(OU\), or account\) defines a guardrail for what actions the principals can perform\. You still need to attach [identity\-based or resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html) to principals or resources in your organization's accounts to actually grant permissions to them\. When a principal belongs to an account that is a member of an organization, the SCPs contribute to the principal's [effective permissions](#scp-effects-on-permissions)\.

****Topics on this page****
+ [Testing effects of SCPs](#scp-warning-testing-effect)
+ [Maximum size of SCPs](#scp-size-limit)
+ [Effects on permissions](#scp-effects-on-permissions)
+ [Using access data to improve SCPs](#data-from-iam)
+ [Tasks and entities not restricted by SCPs](#not-restricted-by-scp)

### Testing effects of SCPs<a name="scp-warning-testing-effect"></a>

AWS strongly recommends that you don't attach SCPs to the root of your organization without thoroughly testing the impact that the policy has on accounts\. Instead, create an OU that you can move your accounts into one at a time, or at least in small numbers, to ensure that you don't inadvertently lock users out of key services\. One way to determine whether a service is used by an account is to examine the [service last accessed data in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html)\. Another way is to [use AWS CloudTrail to log service usage at the API level](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/how-cloudtrail-works.html)\.

### Maximum size of SCPs<a name="scp-size-limit"></a>

All characters in your SCP count against its [maximum size](orgs_reference_limits.md#min-max-values)\. The examples in this guide show the SCPs formatted with extra white space to improve their readability\. However, to save space if your policy size approaches the maximum size, you can delete any white space, such as space characters and line breaks that are outside quotation marks\.

**Tip**  
Use the visual editor to build your SCP\. It automatically removes extra white space\.

### Effects on permissions<a name="scp-effects-on-permissions"></a>

SCPs are similar to AWS Identity and Access Management \(IAM\) permission policies and use almost the same syntax\. However, an SCP never grants permissions\. Instead, SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit \(OU\)\. For more information, see [Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) in the *IAM User Guide*\. 
+ SCPs ***affect only principals*** that are managed by accounts that are part of the organization\. SCPs don't affect resource\-based policies directly\. They also don't affect users or roles from accounts outside the organization\. For example, consider an Amazon S3 bucket that's owned by account A in an organization\. The bucket policy \(a resource\-based policy\) grants access to users from accounts outside the organization\. Account A has an SCP attached\. That SCP doesn't apply to those outside users\. It applies only to users that are managed by account A in the organization\. 
+ An SCP restricts permissions for principals in member accounts, including each AWS account root user\. Any account has only those permissions permitted by ***every*** parent above it\. If a permission is blocked at any level above the account, either implicitly \(by not being included in an `Allow` policy statement\) or explicitly \(by being included in a `Deny` policy statement\), a user or role in the affected account can't use that permission, even if the account administrator attaches the `AdministratorAccess` IAM policy with \*/\* permissions to the user\.
+ Users and roles must still be granted permissions with appropriate IAM permission policies\. A user without any IAM permission policies has no access, even if the applicable SCPs allow all services and all actions\.
+ If a user or role has an IAM permission policy that grants access to an action that is also allowed by the applicable SCPs, the user or role can perform that action\.
+ If a user or role has an IAM permission policy that grants access to an action that is either not allowed or explicitly denied by the applicable SCPs, the user or role can't perform that action\.
+ SCPs affect all users and roles in attached accounts, ***including the root user***\. The only exceptions are those described in [Tasks and entities not restricted by SCPs](#not-restricted-by-scp)\.
+ SCPs ***do not ***affect any service\-linked role\. Service\-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs\.
+ When you disable the SCP policy type in a root, all SCPs are automatically detached from all AWS Organizations entities in that root\. AWS Organizations entities include organizational units, organizations, and accounts\. If you reenable SCPs in a root, that root reverts to only the default `FullAWSAccess` policy automatically attached to all entities in the root\. Any attachments of SCPs to AWS Organizations entities from before SCPs were disabled are lost and aren't automatically recoverable, although you can manually reattach them\.
+ If both a permissions boundary \(an advanced IAM feature\) and an SCP are present, then the boundary, the SCP, and the identity\-based policy must all allow the action\.

### Using access data to improve SCPs<a name="data-from-iam"></a>

When signed in with master account credentials, you can view [service last accessed data](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) for an AWS Organizations entity or policy in the **AWS Organizations** section of the IAM console\. You can also use the AWS Command Line Interface \(AWS CLI\) or AWS API in IAM to retrieve service last accessed data\. This data includes information about which allowed services that principals in an AWS Organizations account last attempted to access and when\. You can use this information to identify unused permissions so that you can refine your SCPs to better adhere to the principle of [least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)\.

For example, you might have a [deny list SCP](orgs_manage_policies_scps_strategies.md#orgs_policies_denylist) that prohibits access to three AWS services\. All services that aren't listed in the SCP's `Deny` statement are allowed\. The service last accessed data in IAM tells you which AWS services are allowed by the SCP but are never used\. With that information, you can update the SCP to deny access to services that you don't need\.

For more information, see the following topics in the *IAM User Guide*:
+ [Viewing Organizations Service Last Accessed Data for Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-view-data-orgs.html)
+ [ Using Data to Refine Permissions for an Organizational Unit](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-example-scenarios.html#access_policies_access-advisor-reduce-permissions-orgs) 

### Tasks and entities not restricted by SCPs<a name="not-restricted-by-scp"></a>

You ***can't*** use SCPs to restrict the following tasks:
+ Any action performed by the master account
+ Any action performed using permissions that are attached to a service\-linked role
+ Register for the Enterprise support plan as the root user
+ Change the AWS support level as the root user
+ Manage Amazon CloudFront keys
+ Provide trusted signer functionality for CloudFront private content
+ Modify AWS account email allowance/reverse DNS
+ Tasks on some AWS\-related services:
  + Alexa Top Sites
  + Alexa Web Information Service
  + Amazon Mechanical Turk
  + Amazon Product Marketing API

**Exceptions for only member accounts created before September 15, 2017**  
For **some** accounts created *before* September 15, 2017, you ***can't ***use SCPs to prevent the root user in those member accounts from performing the following four tasks:

**Important**  
For **all** accounts created *after* September 15, 2017, these exceptions do not apply and you ***can*** use SCPs to prevent the root user in those member accounts from performing the following four tasks\. However, unless you are certain that **all** of the accounts in your organization were created after September 15, 2017, we recommend that you don’t rely on SCPs to try to restrict these operations\.
+ Enable or disable multi\-factor authentication on the root user
+ Create, update, or delete x\.509 keys for the root user
+ Change the root user's password \(only on some accounts \- see following **Important** note\)
+ Create, update, or delete root access keys \(only on some accounts \- see following **Important** note\)