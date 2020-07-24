# Creating and updating tag policies<a name="orgs_manage_policies_tag-policies-create"></a>

After you enable tag policies for your organization, you can create a tag policy\.

**Important**  
Untagged resources don't appear as noncompliant in results\.

## Creating a tag policy<a name="create-tag-policy-procedure"></a>

To create tag policies, you need permission to run the following action:
+ `organizations:CreatePolicy`

**To create a tag policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Policies** tab, choose **Tag policies**\.

1. On the **Tag policies** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a name and description for the policy\.

   You can build the tag policy using the **Visual editor** as described in this procedure\. You can also type or paste a tag policy in the **JSON** tab\. For information on tag policy syntax, see [Tag policy syntax](orgs_manage_policies_example-tag-policies.md#tag-policy-syntax-reference)\.

1. For **Tag Key**, specify the name of a tag key to add\. 

1. For **Tag key capitalization compliance**, leave this option cleared \(the default\) to specify that the parent tag policy should define the case treatment for the tag key\. 

   Select this option only if you want to specify different capitalization for the tag key\. If you select this option, the capitalization you specified for **Tag Key** overrides the case treatment specified in a parent policy\. 

   If a parent policy doesn't exist and you don't select this option, tag keys in all lowercase characters are considered compliant\. For more information about parent policies, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.
**Tip**  
Consider using the example tag policy shown in [Example 1: Define organization\-wide tag key case](orgs_manage_policies_example-tag-policies.md#tag-policy-example-key-case) as a guide in creating a tag policy that define tag keys and their case treatment\. Attach it to the organization root\. Later, you can create and attach additional tag policies to OUs or accounts to create additional tagging rules\. 

1. For **Tag value compliance**, select the check box if you want to add allowed values for this tag key\.

   By default, this option is cleared, which means that only any values inherited from a parent policy are considered compliant\. If a parent policy doesn't exist or specify tag values, any value \(including no value at all\) is considered compliant\. 

   To update the list of acceptable tag values, select **Specify allowed values for this tag key** and then **Specify values**\. When prompted, enter the new values and choose **Save changes**\.

1. For **Prevent noncompliant operations for this tag**, leave this option cleared \(the default\) unless you are experienced with using tag policies\. Make sure that you have reviewed the recommendations in [Understanding enforcement](orgs_manage_policies_tag-policies-enforcement.md)\. Otherwise, you could prevent users in your organization's accounts from tagging the resources they need\. 

   If you do want to enforce compliance with this tag key, select the check box and then **Specify allowed values**\. When prompted, enter the resource types to add\. Then choose **Save changes**\.

1. \(Optional\) To add another tag key to this tag policy, choose **Add tag key**\. Then perform steps 5–8 to define the tag key\.

1. When you're finished building your tag policy, choose **Save changes**\.

**To create a tag policy \(AWS CLI, AWS API\)**  
You can use one of the following to create a tag policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  For the complete procedure for using tag policies in the AWS CLI, see [Using tag policies in the AWS CLI](tag-policy-cli.md)\.
+ AWS API: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

**What to Do Next**  
After you create a tag policy, you can put your tagging rules into effect\. To do that, [attach the policy ](attach-tag-policy.md) to the organization root, organizational units \(OUs\), AWS accounts within your organization, or a combination of organization entities\. 

## Updating a tag policy<a name="update-tag-policy-procedure"></a>

To update a tag policy, you must have permission to run the following actions:
+ `organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
+ `organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

**To update a tag policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Policies** tab, choose **Tag policies**\.

1. On the **Tag policies** page, choose the tag policy that you want to update\.

1. In the details pane on the right, choose **View details**\. 

1. On the page that shows the tag policy, choose **Edit policy**\.

1. Make your changes either by using the visual editor or by editing the JSON\. 

1. When you're finished updating the tag policy, choose **Save changes**\.

**To update a policy \(AWS CLI, AWS API\)**  
You can use one of the following to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)
+ AWS API: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

## Deleting a tag policy<a name="delete-tag-policy-procedure"></a>

When signed in to your organization's master account, you can delete a policy that you no longer need in your organization\. 

Before you can delete a policy, you must first detach it from all attached entities\.

To delete a tag policy, you must have permission to run the following action:
+ `organizations:DeletePolicy`

**To delete a tag policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. The tag policy that you want to delete must first be detached from all roots, OUs, and accounts\. Follow the steps in [Detaching a tag policy](attach-tag-policy.md#detach-tag-policy) to detach the tag policy from all entities in the organization\.

1. On the **Policies** tab, choose **Tag policies**\.

1. From the **Tag policies** page, choose the tag policy that you want to delete\. 

1. Choose **Delete policy**\.

**To delete a tag policy \(AWS CLI, AWS API\)**  
You can use one of the following to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)
+ AWS API: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)