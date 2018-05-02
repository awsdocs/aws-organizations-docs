# Service Control Policies<a name="orgs_manage_policies_scp"></a>

Service control policies \(SCPs\) are one type of policy that you can use to manage your organization\. SCPs enable you to restrict, at the account level of granularity, what services and actions the users, groups, and roles in those accounts can do\. For additional details about SCPs, strategies for using them, and their syntax, see [About Service Control Policies](orgs_manage_policies_about-scps.md)\.

SCPs are available only in an organization that has [all features enabled](orgs_manage_org_support-all-features.md)\. SCPs are not available if your organization has enabled only the consolidated billing features\.

SCPs are similar to IAM permission policies and use almost the exact same syntax\. However, an SCP never grants permissions\. Instead, think of an SCP as a "filter" that enables you to restrict what service and actions can be accessed by users and roles in the accounts that you attach the SCP to\. An SCP that is applied at the root cascades its permissions to the OUs below it\. An OU at the next level down gets the mathematical intersection of the permissions that flow down from the parent root and the SCPs that are attached to the OU\. In other words, any account has only those permissions permitted by ***every*** parent above it\. If a permission is blocked at any level above the account, either implicitly \(by not being included in an "Allow" policy statement\) or explicitly \(by being included in a "Deny" policy statement\) then a user or role in the affected account cannot use that permission, even if the account administrator attaches the **AdministratorAccess** IAM policy with \*/\* permissions to the user\.

**Warning**  
We strongly recommend that you do not attach SCPs to the root of your organization without thoroughly testing the impact that the policy has on accounts\. Instead, create an OU that you can move your accounts into one at a time, or at least in small numbers, to ensure that you don't inadvertently lock users out of key services\. One way to determine whether a service is used by an account is to examine the [service last accessed data in IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html)\. Another way is to [use AWS CloudTrail to log service usage at the API level](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/how-cloudtrail-works.html)\.

**Note**  
All characters that you type count against the [size limit of your SCP](orgs_reference_limits.md#min-max-values)\. The examples in this guide show the SCPs formatted with extra whitespace to improve their readability\. However, you can delete any whitespace, such as space characters and line breaks that are outside of quotation marks, to save space if your policy size approaches the limit\.

**Considerations**
+ Users and roles must still be granted permissions with appropriate IAM permission policies\. A user without any IAM permission policies has no access at all, even if the applicable SCPs allow all services and all actions\.
+ Remember that SCPs are filters that *limit* what access can be exercised in an account\.
+ SCPs do not grant permissions to any user or role\.
+ If a user or role has an IAM permission policy that grants access to an action that is also allowed by the applicable SCPs, then the user or role can perform that action\.
+ If a user or role has an IAM permission policy that grants access to an action that is either not allowed or explicitly denied by the applicable SCPs, then the user or role cannot perform that action\.

**Important**  
SCPs affect all users and roles in attached accounts, ***including the root user***\.The only exceptions are those described in the following list of tasks that are not affected and cannot be restricted by using SCPs\.
SCPs ***do not ***affect any service\-linked role\. Service\-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs\.
SCPs ***affect only principals*** that are managed by accounts that are part of the organization\. They do not affect users or roles from accounts outside the organization\. For example, consider an S3 bucket that is owned by account "A" in an organization\. The bucket policy grants access to users from accounts outside of the organization\. Account "A" has an SCP attached\. That SCP does not apply to those outside users\. It applies only to users that are managed by account "A" in the organization\. 
When you disable the SCP policy type in a root, all SCPs are automatically detached from all entities in that root\. If you re\-enable SCPs in a root, that root reverts to only the default `FullAWSAccess` policy automatically attached to all entities in the root\. Any attachments of SCPs to entities from before SCPs were disabled are lost and are not automatically recoverable, although you can manually reattach them\.

**Tasks and entities not restricted by SCPs**
+ Any action performed using permissions that are attached to a service\-linked role\.
+ Managing root credentials\. No matter what SCPs are attached, the root user in an account can always do the following:
  + Changing the root user's password
  + Creating, updating, or deleting root access keys
  + Enabling or disabling multi\-factor authentication on the root user
  + Creating, updating, or deleting x\.509 keys for the root user
+ Registering for the Enterprise Support plan as the root user
+ Closing an account as the root user \(from within the account instead of submitting a ticket to AWS Support\)
+ Changing the AWS support level as the root user
+ Managing CloudFront keys
+ Trusted signer functionality for CloudFront private content
+ Modifying AWS account email allowance/rDNS
+ Performing tasks on some AWS\-related services:
  + Alexa Top Sites
  + Alexa Web Information Service
  + Amazon Mechanical Turk
  + Amazon Product Marketing API

You can use the procedures in the following sections to create and update service control policies \(SCP\)\. 

To learn more about policy types, see [Managing Organization Policies](orgs_manage_policies.md)\.

## Creating a Service Control Policy<a name="create_policy"></a>

When signed in with permissions to your organization's master account, you can create an SCP\. To create an SCP, complete the following steps\.

**Minimum permissions**  
To create a policy within your organization, you must have the following permissions:  
`organizations:CreatePolicy`

**To create a service control policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Policies** tab, choose **Create Policy**\.

1. Choose the technique that you want to use to create the policy:
   + **Policy generator** \- Select services and actions from a list\. The generator uses your choices to create an SCP for you\.
   + **Copy an existing SCP** \- Make a copy of an existing SCP and customize it to meet your new requirements, or create a new SCP using a policy text editor\.

1. If you chose **Policy generator**, follow these steps:

   1. Type a name and a description for the SCP that will help you find it and remember its intended purpose later\.

   1. Choose an **Effect**\. The effect applies only to this one policy statement\. Each statement can have its own effect\.
      + Specify **Deny** if you want to create a [blacklist that blocks all access to the specified services and actions](orgs_getting-started_concepts.md#blacklisting)\. This kind of policy is intended to be used in addition to a policy like `FullAWSAccess` that grants permissions\. The explicit Deny on specific actions in the blacklist policy overrides the Allow in any other policy\.
      + Specify **Allow** if you want to create a [whitelist that specifies the services and actions to which users can be granted access](orgs_getting-started_concepts.md#whitelisting)\. Remember that by default there is already a `FullAWSAccess` policy attached to every root, OU, and account that grants all permissions\. So if you want to allow a more limited set of services and actions, you must replace the default policy with a new policy that allows fewer permissions\.

   1. In the **Statement builder**, select the service whose actions you want to either allow or deny\.

   1. Also in the **Statement builder**, select the actions in that service that you want to specify in this policy\. You can choose **Select All**, or select multiple actions from the list\.

   1. Choose **Add statement** to add it to the current SCP under construction\. 

   1. Repeat the preceding two steps as often as you need to add more statements to the SCP\.

   1. When your SCP includes all the statements that you need, choose **Create policy** to save the completed SCP\.

1. If you instead chose **Copy an existing SCP**, follow these steps:
**Note**  
You can use this option to create a new SCP using the editor\. Don't select a policy to copy—just fill in the name and type your desired policy text\.

   1. Choose **Select a policy**, and then choose the SCP that you want to copy to use as a starting point\. You can type in the **Filter** box to narrow your choices if the list is long\.

   1. For **Policy name** and **Description**, type a name and a description for the policy that will help you find it and remember its intended purpose later\.

   1. In **Edit policy**, edit the copy of the SCP to meet your new requirements\. If you did not choose an SCP from the list, then this box is empty and you can type a completely new policy\.

   1. When you are done, choose **Create** to save your completed SCP\.

   Your new SCP appears in the list of the organization's policies\. You can now [attach your SCP to the root, OUs, or accounts](orgs_manage_policies.md#attach_policy)\.

**To create a service control policy \(AWS CLI, AWS API\)**  
You can use the following commands to create an SCP:
+ AWS CLI: [aws organizations create\-policy](http://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)
+ AWS API: [CreatePolicy](http://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

## Updating a Service Control Policy<a name="update_policy"></a>

When signed in to your organization's master account, you can rename or change the contents of a policy\. To do this, complete the following steps\.

**Note**  
Changing the contents of an SCP immediately affects any users, groups, and roles in all attached accounts\.

**Minimum permissions**  
To update a policy in your AWS organization, you must have the following permissions:  
`organizations:UpdatePolicy`
`organizations:DescribeOrganization`

**To update a policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Choose the **Policies** tab\.

1. Choose the policy that you want to update\.

1. In the details pane on the right, choose **Policy editor**\.

   The editor window opens initially in read\-only mode in the center pane\.

1. Choose **Edit** to enable making changes to the policy\.

1. Make your changes, and then choose **Save**\.

**To update a policy \(AWS CLI, AWS API\)**  
You can use the following commands to update a policy: 
+ AWS CLI: [aws organizations update\-policy](http://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)
+ AWS API: [UpdatePolicy](http://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)