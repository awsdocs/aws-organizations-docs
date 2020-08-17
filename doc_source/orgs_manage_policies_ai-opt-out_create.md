# Creating, updating, and deleting AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_create"></a>

After you enable AI services opt\-out policies for your organization, you can create a policy\.

## Creating an AI services opt\-out policy<a name="create-ai-opt-out-policy-procedure"></a>

**Minimum permissions**  
To create an AI services opt\-out policy, you need permission to run the following action:  
`organizations:CreatePolicy`

**To create an AI services opt\-out policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. In the organization's master account, sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. On the **Policies** tab, choose **AI services opt\-out policies**\.

1. On the **AI services opt\-out policies** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a name and description for the policy\.

1. Type or paste the policy text in the **JSON** tab\. For information about AI services opt\-out policy syntax, see [AI services opt\-out policy syntax and examples](orgs_manage_policies_ai-opt-out_syntax.md)\. For example policies that you can use as a starting point, see [AI services opt\-out policy examples](orgs_manage_policies_ai-opt-out_syntax.md#ai-opt-out-policy-examples)\.

1. When you're finished editing your policy, choose **Create policy** at the lower\-right corner of the page\.

**To create an AI services opt\-out policy \(AWS CLI, AWS API\)**  
You can use one of the following to create a tag policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  For examples that use the AWS Command Line Interface \(AWS CLI\) to create an AI services opt\-out policy, see [Using the AWS CLI to manage AI services opt\-out policies](orgs_manage_policies_ai-opt-out_cli.md)\.
+ AWS API: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

**What to do next**  
After you create an AI services opt\-out policy, you can put your opt\-out choices into effect\. To do that, you can [attach the policy ](attach-tag-policy.md) to the organization root, organizational units \(OUs\), AWS accounts within your organization, or a combination of all of those\. 

## Updating an AI services opt\-out policy<a name="update-tag-policy-procedure"></a>

To update an AI services opt\-out policy, you must have permission to run the following actions:
+ `organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
+ `organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the Amazon Resource Name \(ARN\) of the specified policy \(or "\*"\)

**To update an AI services opt\-out policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. In the organization's master account, sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. On the **Policies** tab, choose **AI services opt\-out policies**\.

1. On the **AI services opt\-out policies** page, choose the policy that you want to update\.

1. In the details pane on the right, choose **View details**\. 

1. On the page that shows the tag policy, choose **Edit policy**\.

1. Make your changes either by editing the JSON\. 

1. When you're finished updating the policy, choose **Save changes**\.

**To update a policy \(AWS CLI, AWS API\)**  
You can use one of the following to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)
+ AWS API: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

## Deleting an AI services opt\-out policy<a name="delete-tag-policy-procedure"></a>

When signed in to your organization's master account, you can delete a policy that you no longer need in your organization\. 

Before you can delete a policy, you must first detach it from all attached entities\.

To delete a policy, you must have permission to run the following action:
+ `organizations:DeletePolicy`

**To delete an AI services opt\-out policy \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. In the organization's master account, sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. The policy that you want to delete must first be detached from all roots, OUs, and accounts\. Follow the steps in [Detaching an AI services opt\-out policy](orgs_manage_policies_ai-opt-out_attach.md#orgs_manage_policies_ai-opt-out_detach) to detach the policy from all entities in the organization\.

1. On the **Policies** tab, choose **AI services opt\-out policies**\.

1. From the **AI services opt\-out policies** page, choose the policy that you want to delete\. 

1. Choose **Delete policy**\.

**To delete an AI services opt\-out policy \(AWS CLI, AWS API\)**  
You can use one of the following to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)
+ AWS API: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)