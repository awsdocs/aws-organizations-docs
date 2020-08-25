# Creating and updating SCPs<a name="orgs_manage_policies_scps_create"></a>

When signed in with permissions to your organization's master account, you can create and update [service control policies \(SCPs\)](orgs_manage_policies_scps.md)\. You create SCPs by building statements that deny or allow access to services and actions that you specify\.

The default configuration for working with SCPs is to create statements that deny access\. With deny statements, you can also specify resources and conditions for the statement and use the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element\. For allow statements, you can specify services and actions only\.

For more information about statements that deny access and allow access, see [Strategies for using SCPs](orgs_manage_policies_scps_strategies.md)\.

**Tip**  
You can use [service last accessed data](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) in [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) to update your SCPs to restrict access to only the AWS services that you need\. For more information, see [Viewing Organizations Service Last Accessed Data for Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-view-data-orgs.html) in the *IAM User Guide\.* 

## Creating an SCP<a name="create-an-scp"></a>

To create SCPs, you need permission to run the following action:
+ `organizations:CreatePolicy`

**To create a service control policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Policies** tab, choose **Service control policies**\. 

1. On the **Service control policies** page, choose **Create policy**\.

1. On the **Create policy** page, enter a name and description for the policy\.

   To build the policy, your next steps vary depending on whether you want to add a statement that denies or allows access\. With deny statements, you have additional control because you can restrict access to specific resources, define conditions for when SCPs are in effect, and use the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element\. For more information, see [SCP syntax](orgs_manage_policies_scps_syntax.md)\.

1. To add a statement that *denies* access:

   1. In the left pane of the **Policy** section, choose the service to add actions for\.

      As you choose options on the left, the right pane updates to show the JSON policy\. You can also type or paste policies in the editor in the **Policy** section's right pane\. However, this procedure describes how to use the visual editor on the left to build your SCP\. 

   1. From the list that opens of available actions for that service, choose the action or actions to deny\. 

   1. Specify resources to include in the statement\. 
      + Choose **Add resource**\.
      + On the **Add resource** screen, choose the service from the list and then choose the **Resource type**\. Enter the Amazon Resource Name \(ARN\) in **Resource ARN**\.
      + Choose **Add resource**\.
**Tip**  
The resource element is required\. If you want to specify all resources for the selected service, edit the resource statement in the right pane to read `"Resource":"*"`\.

   1. \(Optional\) To specify conditions for when a policy is in effect, choose **Add condition**\. For the selected service, specify the following:
      + **Condition key** – You can specify a condition key that is available for all AWS services \(for example, `aws:SourceIp`\) or a service\-specific key \(for example, `ec2:InstanceType`\)\. 
      + **Qualifier** – \(Optional\) If you enter multiple values for the condition, you can specify a [qualifier](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html) for testing requests against the values\.
      + **Operator** – You can use [operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) to restrict access based on comparing a key to a value\. 

        For any condition operator except the `Null` condition, you can choose the [IfExists](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_IfExists) option\. 
      + **Value** – \(Optional\) Specify one or more values for the condition\.

      Choose **Add condition**\.

      For more information about condition keys, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\. 

   1. \(Optional\) To use the `NotAction` element to deny access to all of the listed resources except for specified actions, replace `Action` in the left pane with `NotAction`, just after the `"Effect": "Deny",` element\. For more information, see [IAM JSON Policy Elements: NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) in the *IAM User Guide*\. 

1. To add a statement that *allows* access:

   1. In the right pane of the **Policy** section, change `"Effect": "Deny"` to `"Effect": "Allow"`\.

   1. In the left pane of the **Policy** section, choose the service to add actions for\.

      As you choose options on the left, the right pane updates to show the JSON policy\. You can also type or paste policies in the editor in the **Policy** section's right pane\. However, this procedure describes how to use the visual editor on the left to build your SCP\. 

   1. From the list that opens of available actions for that service, choose the action or actions to allow\. 

1. \(Optional\) To add another statement to the policy, choose **Add statement** and use the visual editor to build the next statement\. 

1. When you're finished adding statements, choose **Create policy** to save the completed SCP\.

Your new SCP appears in the list of the organization's policies\. You can now [attach your SCP to the root, OUs, or accounts](orgs_manage_policies_scps_attach.md)\.

**Note**  
SCPs don't take effect on the master account and in a few other situations\. For more information, see [Tasks and entities not restricted by SCPs](orgs_manage_policies_scps.md#not-restricted-by-scp)\.

**To create a service control policy \(AWS CLI, AWS API\)**  
You can use one of the following commands to create an SCP:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)
+ AWS API: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

## Updating an SCP<a name="update_policy"></a>

When signed in to your organization's master account, you can rename or change the contents of a policy\. Changing the contents of an SCP immediately affects any users, groups, and roles in all attached accounts\.

To update an SCP, you need permission to run the following actions:
+ `organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
+ `organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

**To update a policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Choose the **Policies** tab\. 

1. On the **Policies** tab, choose **Service control policies**\.

1. Choose the policy that you want to update\.

1. In the details pane on the right, choose **Policy editor**\.

1. Choose **Edit** to enable making changes to the policy\.

1. Make your changes by editing the policy in the right pane\. For deny statements, you can also use the visual editor in the left pane to make changes\. When you're finished, choose **Save changes**\.

**To update a policy \(AWS CLI, AWS API\)**  
You can use one of the following commands to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)
+ AWS API: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

### For more information<a name="orgs_create_policies_scp-more-info"></a>

For more information about creating SCPs, see the following topics:
+ [Example service control policies](orgs_manage_policies_scps_examples.md)
+ [SCP syntax](orgs_manage_policies_scps_syntax.md)

## Deleting a policy<a name="delete_policy"></a>

When signed in to your organization's master account, you can delete a policy that you no longer need in your organization\. 

**Notes**  
Before you can delete a policy, you must first detach it from all attached entities\. 
You can't delete any AWS managed SCP such as the SCP named `FullAWSAccess`\.

To delete an SCP, you need permission to run the following action:
+ `organizations:DeletePolicy`

**To delete a policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. The policy that you want to delete must first be detached from all roots, OUs, and accounts\. Follow the steps in [Detaching an SCP from the organization root, OUs, or accounts](orgs_manage_policies_scps_attach.md#detach_policy) to detach the policy from all entities in the organization\.

1. On the **Policies** tab, choose **Service control policies**\.

1. On the **Service control policies** page, choose the SCP to delete\.

1. Choose **Delete policy**\.

**To delete a policy \(AWS CLI, AWS API\)**  
You can use one of the following commands to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)
+ AWS API: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)