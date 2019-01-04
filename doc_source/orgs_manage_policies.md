# Managing Organization Policies<a name="orgs_manage_policies"></a>

Policies in AWS Organizations enable you to apply additional types of management to the AWS accounts in your organization\. Policies are enabled only after you [enable all features in your organization](orgs_manage_org_support-all-features.md)\. You can apply policies to the following entities in your organization:
+ **A root** – A policy applied to a root applies to all accounts in the organization
+ **An OU** – A policy applied to an OU applies to all accounts in the OU and to any child OUs
+ **An account** – A policy applied to an account applies only to that one account

**Notes**  
Service control policies never apply to the master account, no matter which root or OU the master account is located in\.
Currently, service control policy \(SCP\) is the only supported policy type\.
Policy types are *available* to use in an organization when you [enable all features](orgs_manage_org_support-all-features.md)\. However, at the root level, you can disable an individual policy type using the [EnablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnablePolicyType.html) and [DisablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisablePolicyType.html) operations\. Use the [DescribeOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeOrganization.html) API operation to determine what organization policy types are available to use\. Use the [ListRoots](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListRoots.html) API operation to see which policy types are enabled and disabled in each root\.  
The AWS Organizations console can also display the enabled and disabled policy types\. On the **Organize accounts** tab, choose the `Root` in the navigation pane on the left\. The details pane on the right shows all of the available policy types and indicates which are enabled and which are disabled\.

For procedures that are specific to each type of policy, see the following topics:
+ **[Service control policies](orgs_manage_policies_scp.md)**\. Service control policies \(SCPs\) are similar to IAM permission policies and use almost the exact same syntax\. However, an SCP never grants permissions\. Instead, think of an SCP as a "filter" that enables you to restrict what service and actions can be accessed by users and roles in the accounts that you attach the SCP to\. An SCP applied at the root cascades its permissions to the OUs below it\. An OU at the next level down gets the mathematical intersection of the permissions flowing down from the parent root and the SCPs that are attached to the child OU\. In other words, any account has only those permissions permitted by ***every*** OU and the parent root above it\. If a permission is blocked at any level above the account, either implicitly \(by not being included in an "`Allow`" policy statement\) or explicitly \(by being included in a "`Deny`" policy statement\), a user or role in the affected account can't use that permission, even if the account administrator attaches the `AdministratorAccess` IAM policy with \*/\* permissions to the user\.

**Important**  
When you disable a policy type in a root, all policies of that type are automatically detached from all entities in that root\. If you reenable the policy type, that root reverts to the default state for that policy type\. For example, if you reenable SCPs in a root, all entities in that root are initially attached only to the default SCP `FullAWSAccess` policy\. Any attachments of policies to entities from before the policy type was disabled are lost and aren't automatically recoverable\.

The following procedures apply to ***all*** policy types\. You must [enable a policy type in a root](#enable_policies_on_root) before you can attach policies of that type to any entities in that root\. 

**Topics**
+ [Listing and Displaying Information about Organization Policies](#list-policy-details)
+ [Editing a Policy](#edit-pol)
+ [Enabling and Disabling a Policy Type on a Root](#enable_policies_on_root)
+ [Attaching a Policy to Roots, OUs, or Accounts](#attach_policy)
+ [Detaching a Policy from Roots, OUs, or Accounts](#detach_policy)
+ [Deleting a Policy](#delete_policy)
+ [Service Control Policies](orgs_manage_policies_scp.md)

## Listing and Displaying Information about Organization Policies<a name="list-policy-details"></a>

This section describes various ways to get details about the policies in your organization\.

### Listing All Policies in the Organization<a name="list-all-pols-in-org"></a>

**Minimum permissions**  
To list the policies within your organization, you must have the following permission:  
`organizations:ListPolicies`<a name="proc-list-all-pols-in-org"></a>

**To list all policies in the organization \(Console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Choose the **Policies** tab\.

   The displayed list includes the policies of all types that are currently defined in the organization\.

**To list all policies in an organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to list policies in an organization:
+ AWS CLI: [aws organizations list\-policies](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies.html)
+ AWS API: [ListPolicies](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPolicies.html)

### Listing All Policies Attached to a Root, OU, or Account<a name="list-all-pols-in-entity"></a>

**Minimum permissions**  
To list the policies that are attached to a root, OU, or account within your organization, you must have the following permission:  
`organizations:ListPoliciesForTarget` with a `Resource` element in the same policy statement that includes the ARN of the specified target \(or "\*"\)

**To list all policies that are attached directly to a specified root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate](orgs_manage_ous.md#navigate_tree) to the root, OU, or account whose policy attachments you want to see\.

   1. For a root or OU, do not select any check boxes\. This way, the details pane on the right shows the information about the root or OU that you are viewing\. Alternatively, you can navigate to the parent of the OU, and then select the check box for the OU whose information you want to see\.

   1. For an account, check the box for the account\.

1. In the details pane on the right, expand the **Service control policies** section\.

   The displayed list shows all policies that are attached directly to this entity\. It also shows policies that affect this entity because of inheritance from the root or a parent OU\.

**To list all policies that are attached directly to a specified root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following commands to list policies that are attached to an entity:
+ AWS CLI: [aws organizations list\-policies\-for\-target](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies-for-target.html)
+ AWS API: [ListPoliciesForTarget](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPoliciesForTarget.html)

### Listing All Roots, OUs, and Accounts That a Policy Is Attached To<a name="list-all-entities-attached-to-pol"></a>

**Minimum permissions**  
To list the entities that a policy is attached to, you must have the following permission:  
`organizations:ListTargetsForPolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

**To list all roots, OUs, and accounts that have a specified policy attached \(console\)**

1. Choose the **Policies** tab, and select the check box next to the policy that you're interested in\.

1. In the details pane on the right, choose one of the following:
   +  **Accounts** to see the list of accounts that the policy is directly attached to
   + **Organizational units** to see the list of OUs that the policy is directly attached to
   + **Roots** to see the list of roots that the policy is directly attached to

**To list all roots, OUs, and accounts that have a specified policy attached \(AWS CLI, AWS API\)**  
You can use one of the following commands to list entities that have a policy:
+ AWS CLI: [aws organizations list\-targets\-for\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-targets-for-policy.html)
+ AWS API: [ListTargetsForPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListTargetsForPolicy.html)

### Getting Details About a Policy<a name="get-details-about-pol"></a>

**Minimum permissions**  
To display the details of a policy, you must have the following permission:  
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

**To get details about a policy \(console\)**

1. Choose the **Policies** tab and select the check box next to the policy that you're interested in\.

   The Details pane on the right displays the available information about the policy, including its ARN, description, and attachments\.

1. To view the content of the policy, choose **Policy editor**\.

   The center pane shows the following information:
   + The details about the policy: its name, description, unique ID, and ARN\.
   + The list of roots, OUs, and accounts that the policy is attached to\. Choose each item to see the individual entities of each type\.
   + The policy's content \(specific to the type of policy\):
     + For SCPs, the JSON text that defines the permissions that are allowed in attached accounts

     To update the contents of the policy document, choose **Edit** \. Choose **Save** when you are done\. For more details, see the next section\.

**To get details about a policy \(AWS CLI, AWS API\)**  
You can use one of the following commands to get details about a policy:
+ AWS CLI: [aws organizations describe\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-policy.html)
+ AWS API: [DescribePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribePolicy.html)

## Editing a Policy<a name="edit-pol"></a>

**Minimum permissions**  
To display the details of a policy, you must have the following permissions:  
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
`organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

## Enabling and Disabling a Policy Type on a Root<a name="enable_policies_on_root"></a>

Before you can attach a policy of any type to a root, you must first enable that root to support the specified type of policy\. 

**Note**  
Currently, you can have only one root in an organization\.
Currently, the only supported policy type is SCP\.

**Important**  
When you disable a policy type in a root, all policies of that type are automatically detached from all entities in that root\. If you reenable the policy type, that root reverts to the default state for that policy type\. For example, if you reenable SCPs in a root, all entities in that root are initially attached to only the default `FullAWSAccess` policy\. Any attachments of policies to entities from before the policy type was disabled are lost and aren't automatically recoverable\.

**Note**  
The AWS Organizations console can display the enabled and disabled status of each policy type\. On the **Organize accounts** tab, choose the `Root` in the left navigation pane\. The details pane on the right side of the screen shows all of the available policy types available and indicates which are enabled and which are disabled in that root\. If the option to **Enable** a type is present, that type is currently disabled\. If the option to **Disable** a type is present, that type is currently enabled\.

**Minimum permissions**  
To enable a policy type in a root in your organization, you must have the following permissions:  
`organizations:EnablePolicyType`
`organizations:DescribeOrganization`

**To enable or disable a policy type on a root \(console\)**

When you sign in to your organization's master account, you can enable or disable policy types on a root\. 

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, choose **Root** in the left navigation pane\.

1. In the details pane on the right side of the screen, next to **Service control policies**, choose **Enable** or **Disable**\.
**Note**  
You must first detach all policies of the specified type from all entities in a root before you can disable the policy type in that root\.

**To enable or disable a policy type on a root \(AWS CLI, AWS API\)**  
You can use one of the following commands to disable a policy type:
+ AWS CLI: [aws organizations enable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-policy-type.html) and [aws organizations disable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-policy-type.html)
+ AWS API: [EnablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnablePolicyType.html) and [DisablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisablePolicyType.html)

## Attaching a Policy to Roots, OUs, or Accounts<a name="attach_policy"></a>

When signed in to your organization's master account, you can attach a policy that you previously created to the root, to an OU, or directly to an account\. To attach a policy, complete the following steps\.

**Minimum permissions**  
To attach a policy to a root, OU, or account, you must have the following permission:  
`organizations:AttachPolicy` with a `Resource` element in the same policy statement that includes "\*" or the ARN of the specified policy and the ARN of the root, OU, or account that you want to attach the policy to

**To attach a policy to a root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the root, OU, or account you want to attach the policy to\.

1. In the **Details** pane on the right, expand the **CONTROL POLICIES** section to see the list of the currently attached policies, and then choose **Attach policy**\.

1. On the list of available policies, find the one that you want and choose **Attach**\. The list of attached policies is updated with the new addition\. The policy goes into effect immediately\. For example, an SCP immediately affects the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU\.

**To attach a policy to a root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following commands to attach a policy:
+ AWS CLI: [aws organizations attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)
+ AWS API: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

## Detaching a Policy from Roots, OUs, or Accounts<a name="detach_policy"></a>

When signed in to your organization's master account, you can detach a policy from the root, OU, or account that it is attached to\. After you detach a policy from an entity, that policy no longer applies to any account that was affected by the now detached entity\. To detach a policy, complete the following steps\. 

**Note**  
You can't detach the last SCP from an entity\. There must be at least one SCP attached to all entities at all times\.

**Minimum permissions**  
To detach a policy from a root, OU, or account, you must have the following permission:  
`organizations:DetachPolicy`

**To detach a policy from a root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the root, OU, or account from which you want to detach the policy\.

1. In the **Details** pane on the right, expand the **CONTROL POLICIES** section to see the list of the currently attached policies\. The **Source** field tells you where the policy comes from\. It can be attached directly to the account or OU, or it could be attached to a parent OU or root\.

1. Choose the **X** next to the policy that you want to detach\. The list of attached policies is updated with the chosen policy removed\. The policy change caused by detaching the policy goes into effect immediately\. For example, detaching a SCP immediately affects the permissions of IAM users and roles in the formerly attached account or accounts under the formerly attached root or OU\.

**To detach a policy from a root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following commands to detach a policy:
+ AWS CLI: [aws organizations detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)
+ AWS API: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)

## Deleting a Policy<a name="delete_policy"></a>

When signed in to your organization's master account, you can delete a policy that you no longer need in your organization\. 

**Notes**  
Before you can delete a policy, you must first detach it from all attached entities\. 
You cannot delete any AWS\-managed SCP such as the one named `FullAWSAccess`\.

To delete a policy, complete the following steps\.

**Minimum permissions**  
To delete a policy, you must have the following permission:  
`organizations:DeletePolicy`

**To delete a policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. The policy that you want to delete must first be detached from all roots, OUs, and accounts\. Follow the steps in [Detaching a Policy from Roots, OUs, or Accounts](#detach_policy) to detach the policy from all entities in the organization\.

1. On the **Policies** tab, choose **All policies** and then select the policy that you want to delete\. 

1. Choose **Delete policy**\.

1. In the **Delete policy** dialog box, choose **Delete**\.

**To delete a policy \(AWS CLI, AWS API\)**  
You can use one of the following commands to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)
+ AWS API: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)