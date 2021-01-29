# Creating, updating, and deleting service control policies<a name="orgs_manage_policies_scps_create"></a>

**Note**  
AWS Organizations is introducing a new version of the Organizations management console\. You can switch between the old console and the new console by choosing the link in the notice boxes at the top of the console\. We encourage you to try the new version and let us know what you think\. We want your feedback and read each submission\.

When you sign in to your organization's management account, you can create and update [service control policies \(SCPs\)](orgs_manage_policies_scps.md)\. You create SCPs by building statements that deny or allow access to services and actions that you specify\.

The default configuration for working with SCPs is to use a "block list" strategy where all actions are implicitly allowed except for those actions you want to block by creating statements that deny access\. With deny statements, you can specify resources and conditions for the statement and use the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element\. For allow statements, you can specify services and actions only\. For more information about statements that deny access and allow access, see [Strategies for using SCPs](orgs_manage_policies_scps_strategies.md)\.

**Tip**  
You can use [service last accessed data](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) in [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) as a data point for updating your SCPs to restrict access to only the AWS services that you need\. For more information, see [Viewing Organizations Service Last Accessed Data for Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-view-data-orgs.html) in the *IAM User Guide\.* 

**In this topic:**
+ After you [enable service control policies](orgs_manage_policies_enable-disable.md#enable-policy-type) for your organization, you can [create a policy](#create-an-scp)\.
+ When your SCP requirements change, you can [update an existing policy](#update_policy)\.
+ When you no longer need a policy and after you detach it from all organizational units \(OUs\) and accounts, you can [delete it](#delete_policy)\.

## Creating an SCP<a name="create-an-scp"></a>

**Minimum permissions**  
To create SCPs, you need permission to run the following action:  
`organizations:CreatePolicy`

------
#### [ Old console ]

**To create a service control policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/home#/policies/service-control)** page, choose **Create policy**\. 

1. On the [**Create policy** page](https://console.aws.amazon.com/organizations/home#/policies/service-control/create), enter a **Policy name** and an optional **Policy description**\.

1. \(Optional\) Add one or more tags by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\. For more information, see [Tagging AWS Organizations resources](orgs_tagging.md)\.

1. To build the policy, your next steps vary depending on whether you want to add a statement that [denies](orgs_manage_policies_scps_strategies.md#orgs_policies_denylist) or [allows](orgs_manage_policies_scps_strategies.md#orgs_policies_allowlist) access\. For more information, see [Strategies for using SCPs](orgs_manage_policies_scps_strategies.md)\. If you use `Deny` statements, you have additional control because you can restrict access to specific resources, define conditions for when SCPs are in effect, and use the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element\. For details about syntax, see [SCP syntax](orgs_manage_policies_scps_syntax.md)\.

   To add a statement that *denies* access:

   1. <a name="step.a"></a>In the left pane of the **Policy** section, under **1\. Choose service to add actions for**, choose an AWS service\.

      As you choose options on the left, the right pane updates to show the JSON policy\. You can also enter or paste policies in the editor in the **Policy** section's right pane\. However, this procedure describes how to use the visual editor on the left to build your SCP\. 

   1. After you select a service, a list opens that contains the available actions for that service\. You can choose **All actions**, or choose one or more individual actions to deny\. 

      The JSON on the right updates to include the actions you selected\.

   1. If you want to add actions from additional services, you can choose **All services** at the top of the **Statement** box, and then repeat [the previous two steps](#step.a) as needed\.

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

   1. <a name="step.d"></a>In the left pane of the **Policy** section, choose the service to add actions for\.

      As you choose options on the left, the right pane updates to show the JSON policy\. You can also enter or paste policies in the editor in the **Policy** section's right pane\. However, this procedure describes how to use the visual editor on the left to build your SCP\. 

   1. From the list that opens of available actions for that service, choose the action or actions to allow\. 

   1. If you want to add actions from additional services, you can choose **All services** at the top of the **Statement** box, and then repeat [the previous two steps](#step.d) as needed\.

1. \(Optional\) To add another statement to the policy, choose **Add statement** and use the visual editor to build the next statement\. 

1. When you're finished adding statements, choose **Create policy** to save the completed SCP\.

Your new SCP appears in the list of the organization's policies\. You can now [attach your SCP to the root, OUs, or accounts](orgs_manage_policies_scps_attach.md)\.

------
#### [ New console ]

**To create a service control policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/v2/home/policies/service-control-policy)** page, choose **Create policy**\. 
**Note**  
The graphical SCP editor is currently available only as part of the original console\. When you finish creating the policy, the console will automatically switch back to the new experience\.

1. On the [**Create policy** page](https://console.aws.amazon.com/organizations/home#/policies/service-control/create), enter a **Policy name** and an optional **Policy description**\.

1. \(Optional\) Add one or more tags by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\. For more information, see [Tagging AWS Organizations resources](orgs_tagging.md)\.

1. To build the policy, your next steps vary depending on whether you want to add a statement that [denies](orgs_manage_policies_scps_strategies.md#orgs_policies_denylist) or [allows](orgs_manage_policies_scps_strategies.md#orgs_policies_allowlist) access\. For more information, see [Strategies for using SCPs](orgs_manage_policies_scps_strategies.md)\. If you use `Deny` statements, you have additional control because you can restrict access to specific resources, define conditions for when SCPs are in effect, and use the [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element\. For details about syntax, see [SCP syntax](orgs_manage_policies_scps_syntax.md)\.

   To add a statement that *denies* access:

   1. <a name="step.b"></a>In the left pane of the **Policy** section, under **1\. Choose service to add actions for**, choose an AWS service\.

      As you choose options on the left, the right pane updates to show the JSON policy\. You can also enter or paste policies in the editor in the **Policy** section's right pane\. However, this procedure describes how to use the visual editor on the left to build your SCP\. 

   1. After you select a service, a list opens that contains the available actions for that service\. You can choose **All actions**, or choose one or more individual actions to deny\. 

      The JSON on the right updates to include the actions you selected\.

   1. If you want to add actions from additional services, you can choose **All services** at the top of the **Statement** box, and then repeat [the previous two steps](#step.b) as needed\.

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

   1. <a name="step.c"></a>In the left pane of the **Policy** section, choose the service to add actions for\.

      As you choose options on the left, the right pane updates to show the JSON policy\. You can also enter or paste policies in the editor in the **Policy** section's right pane\. However, this procedure describes how to use the visual editor on the left to build your SCP\. 

   1. From the list that opens of available actions for that service, choose the action or actions to allow\. 

   1. If you want to add actions from additional services, you can choose **All services** at the top of the **Statement** box, and then repeat [the previous two steps](#step.c) as needed\.

1. \(Optional\) To add another statement to the policy, choose **Add statement** and use the visual editor to build the next statement\. 

1. When you're finished adding statements, choose **Create policy** to save the completed SCP\.

Your new SCP appears in the list of the organization's policies\. You can now [attach your SCP to the root, OUs, or accounts](orgs_manage_policies_scps_attach.md)\.

------
#### [ AWS CLI & AWS SDKs ]

**To create a service control policy**  
You can use one of the following commands to create an SCP:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  The following example assumes that you have a file named `Deny-IAM.json` with the JSON policy text in it\. It uses that file to create a new service control policy\.

  ```
  $ aws organizations create-policy \
      --content file://Deny-IAM.json \
      --description "Deny all IAM actions" \
      --name DenyIAMSCP \
      --type SERVICE_CONTROL_POLICY
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/service_control_policy/p-i9j8k7l6m5",
              "Name": "DenyIAMSCP",
              "Description": "Deny all IAM actions",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": false
          },
           "Content": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Sid\":\"Statement1\",\"Effect\":\"Deny\",\"Action\":[\"iam:*\"],\"Resource\":[\"*\"]}]}"
      }
  }
  ```
+ AWS SDKs: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

------

**Note**  
SCPs don't take effect on the management account and in a few other situations\. For more information, see [Tasks and entities not restricted by SCPs](orgs_manage_policies_scps.md#not-restricted-by-scp)\.

## Updating an SCP<a name="update_policy"></a>

When you sign in to your organization's management account, you can rename or change the contents of a policy\. Changing the contents of an SCP immediately affects any users, groups, and roles in all attached accounts\.

**Minimum permissions**  
To update an SCP, you need permission to run the following actions:  
`organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

------
#### [ Old console ]

**To update a policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/home#/policies/service-control)** page, choose the name of the policy that you want to update\.

1. On the policy's detail page, choose **Edit policy**\.

1. Make any or all of the following changes:
   + You can rename the policy by entering a new name in **Policy name**\.
   + You can change the description by entering new text in **Policy description**\.
   + You can edit the policy text by editing the policy in JSON format in the right pane\. For `Deny` statements, you can use the visual editor in the left pane to make changes\.

1. When you're finished, choose **Save changes**\.

------
#### [ New console ]

**To update a policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/v2/home/policies/service-control-policy)** pagechoose the name of the policy that you want to update\.

1. On the policy's detail page, choose **Edit policy**\.
**Note**  
The graphical SCP editor is currently available only as part of the original console\. When you finish creating the policy, the console will automatically switch back to the new experience\.

1. Make any or all of the following changes:
   + You can rename the policy by entering a new name in **Policy name**\.
   + You can change the description by entering new text in **Policy description**\.
   + You can edit the policy text by editing the policy in JSON format in the right pane\. For `Deny` statements, you can use the visual editor in the left pane to make changes\.

1. When you're finished, choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To update a policy**  
You can use one of the following commands to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)

  The following example renames a policy\.

  ```
  $ aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --name "MyRenamedPolicy"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/service_control_policy/p-i9j8k7l6m5",
              "Name": "MyRenamedPolicy",
              "Description": "Blocks all IAM actions",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": false
          },
          "Content": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Sid\":\"Statement1\",\"Effect\":\"Deny\",\"Action\":[\"iam:*\"],\"Resource\":[\"*\"]}]}"
      }
  }
  ```

  The following example adds or changes the description for a service control policy\.

  ```
  $ aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --description "My new policy description"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/service_control_policy/p-i9j8k7l6m5",
              "Name": "MyRenamedPolicy",
              "Description": "My new policy description",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": false
          },
          "Content": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Sid\":\"Statement1\",\"Effect\":\"Deny\",\"Action\":[\"iam:*\"],\"Resource\":[\"*\"]}]}"
      }
  }
  ```

  The following example changes the policy document of the SCP by specifying a file that contains the new JSON policy text\.

  ```
  $ aws organizations update-policy \
      --policy-id p-zlfw1r64 
      --content file://MyNewPolicyText.json
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/service_control_policy/p-i9j8k7l6m5",
              "Name": "MyRenamedPolicy",
              "Description": "My new policy description",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": false
          },
          "Content": "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Sid\":\"AModifiedPolicy\",\"Effect\":\"Deny\",\"Action\":[\"iam:*\"],\"Resource\":[\"*\"]}]}"
      }
  }
  ```
+ AWS SDKs: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

------

### For more information<a name="orgs_create_policies_scp-more-info"></a>

For more information about creating SCPs, see the following topics:
+ [Example service control policies](orgs_manage_policies_scps_examples.md)
+ [SCP syntax](orgs_manage_policies_scps_syntax.md)

## Editing tags attached to an SCP<a name="tag_policy_scp"></a>

When you sign in to your organization's management account, you can add or remove the tags attached to an SCP\. For more information about tagging, see [Tagging AWS Organizations resources](orgs_tagging.md)\. 

**Minimum permissions**  
To edit the tags attached to an SCP in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:DescribePolicy` – required only when using the Organizations console
`organizations:TagResource`
`organizations:UntagResource`

------
#### [ Old console ]

**To edit the tags attached to an SCP**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/home#/policies/service-control)** page choose the policy with the tags that you want to edit\.

1. In the details pane on the right, under **Tags**, choose **Edit tags**\.

1. Make any or all of the following changes:
   + Change the value of a tag by entering a new value over the old one\. You can't directly modify the tag key\. To change a key, you must delete the tag with the old key and then add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. When you're finished, choose **Save changes**\.

------
#### [ New console ]

**To edit the tags attached to an SCP**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/v2/home/policies/service-control-policy)** page choose the name of the policy with the tags that you want to edit\.

1. On the policy details page, choose the **Tags** tab, and then choose**Manage tags**\.

1. Make any or all of the following changes:
   + Change the value of a tag by entering a new value over the old one\. You can't directly modify the tag key\. To change a key, you must delete the tag with the old key and then add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. When you're finished, choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To edit the tags attached to an SCP**  
You can use one of the following commands to edit the tags attached to an SCP:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------

## Deleting an SCP<a name="delete_policy"></a>

When you sign in to your organization's management account, you can delete a policy that you no longer need in your organization\. 

**Notes**  
Before you can delete a policy, you must first detach it from all attached entities\. 
You can't delete any AWS managed SCP such as the SCP named `FullAWSAccess`\.

**Minimum permissions**  
To delete an SCP, you need permission to run the following action:  
`organizations:DeletePolicy`

------
#### [ Old console ]

**To delete an SCP**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/home#/policies/service-control)** page, choose the SCP that you want to delete\.

1. You must first detach the policy that you want to delete from all roots, OUs, and accounts\. In the details pane on the right, choose each of the **Accounts**, **Organizational units**, and **Roots** sections\. If any accounts, OUs, or Roots indicate that the policy is attached, choose **Detach**\. Repeat until you remove all targets\.

1. With the policy still selected, choose **Delete** above the list of policies\.

------
#### [ New console ]

**To delete an SCP**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/v2/home/policies/service-control-policy)** page, choose the name of the SCP that you want to delete\.

1. You must first detach the policy that you want to delete from all roots, OUs, and accounts\. Choose the **Targets** tab, choose the radio button next to each root, OU, or account that is shown in the **Targets** list, and then choose **Detach**\. In the confirmation dialog box, choose **Detach**\. Repeat until you remove all targets\.

1. Choose **Delete** at the top of the page\.

1. On the confirmation dialog box, enter the name of the policy, and then choose **Delete**\.

------
#### [ AWS CLI & AWS SDKs ]

**To delete an SCP**  
You can use one of the following commands to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)

  The following example deletes the specified SCP\.

  ```
  $ aws organizations delete-policy \
      --policy-id p-i9j8k7l6m5
  ```

  This command produces no output when successful\.
+ AWS SDKs: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)

------