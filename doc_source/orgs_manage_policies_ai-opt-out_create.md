# Creating, updating, and deleting AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_create"></a>

After you enable AI services opt\-out policies for your organization, you can create a policy\.

## Creating an AI services opt\-out policy<a name="create-ai-opt-out-policy-procedure"></a>

**Minimum permissions**  
To create an AI services opt\-out policy, you need permission to run the following action:  
`organizations:CreatePolicy`

------
#### [ AWS Management Console ]

**To create an AI services opt\-out policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. On the **Policies** tab, choose **AI services opt\-out policies**\.

1. On the **AI services opt\-out policies** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a name and description for the policy\.

1. \(Optional\)You can add one or more tags to the policy by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\.

1. Enter or paste the policy text in the **JSON** tab\. For information about AI services opt\-out policy syntax, see [AI services opt\-out policy syntax and examples](orgs_manage_policies_ai-opt-out_syntax.md)\. For example policies that you can use as a starting point, see [AI services opt\-out policy examples](orgs_manage_policies_ai-opt-out_syntax.md#ai-opt-out-policy-examples)\.

1. When you're finished editing your policy, choose **Create policy** at the lower\-right corner of the page\.

------
#### [ AWS CLI, AWS API ]

**To create an AI services opt\-out policy**  
You can use one of the following to create a tag policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  For examples that use the AWS Command Line Interface \(AWS CLI\) to create an AI services opt\-out policy, see [Using the AWS CLI to manage AI services opt\-out policies](orgs_manage_policies_ai-opt-out_cli.md)\.
+ AWS API: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

------

**What to do next**  
After you create an AI services opt\-out policy, you can put your opt\-out choices into effect\. To do that, you can [attach the policy ](attach-tag-policy.md) to the organization root, organizational units \(OUs\), AWS accounts within your organization, or a combination of all of those\. 

## Updating an AI services opt\-out policy<a name="update-tag-policy-procedure"></a>

**Minimum permissions**  
To update an AI services opt\-out policy, you must have permission to run the following actions:  
`organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the Amazon Resource Name \(ARN\) of the specified policy \(or "\*"\)

------
#### [ AWS Management Console ]

**To update an AI services opt\-out policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. On the **Policies** tab, choose **AI services opt\-out policies**\.

1. On the **AI services opt\-out policies** page, choose the policy that you want to update\.

1. In the details pane on the right, choose **View details**\. 

1. On the page that shows the tag policy, choose **Edit policy**\.

1. Make your changes either by editing the JSON\. 

1. When you're finished updating the policy, choose **Save changes**\.

------
#### [ AWS CLI, AWS API ]

**To update a policy**  
You can use one of the following to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)
+ AWS API: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

------

## Editing tags attached to an AI services opt\-out policy<a name="tag-ai-opt-out-policy-procedure"></a>

When signed in to your organization's management account, you can add or remove the tags attached to an AI services opt\-out policy\. To do this, complete the following steps\.

**Minimum permissions**  
To edit the tags attached to an AI services opt\-out policy in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only – to navigate to the policy\)
`organizations:DescribePolicy` \(console only – to navigate to the policy\)
`organizations:TagResource`
`organizations:UntagResource`

------
#### [ AWS Management Console ]

**To edit the tags attached to an AI services opt\-out policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. On the **Policies** tab, choose **AI services opt\-out policies**, and then choose the name of the policy with the tags that you want to edit\.

1. In the chosen policy's details pane, choose **EDIT TAGS**\.

1. You can perform any of these actions on this page:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ AWS CLI, AWS API ]

**To edit the tags attached to a AI services opt\-out policy**  
You can use one of the following commands to edit the tags attached to a AI services opt\-out policy:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS API: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------

## Deleting an AI services opt\-out policy<a name="delete-ai-opt-out-policy-procedure"></a>

When signed in to your organization's management account, you can delete a policy that you no longer need in your organization\. 

Before you can delete a policy, you must first detach it from all attached entities\.

**Minimum permissions**  
To delete a policy, you must have permission to run the following action:  
`organizations:DescribePolicy` \(console only – to navigate to the policy\)
`organizations:DeletePolicy`

------
#### [ AWS Management Console ]

**To delete an AI services opt\-out policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. The policy that you want to delete must first be detached from all roots, OUs, and accounts\. Follow the steps in [Detaching an AI services opt\-out policy](orgs_manage_policies_ai-opt-out_attach.md#orgs_manage_policies_ai-opt-out_detach) to detach the policy from all entities in the organization\.

1. On the **Policies** tab, choose **AI services opt\-out policies**\.

1. From the **AI services opt\-out policies** page, choose the policy that you want to delete\. 

1. Choose **Delete policy**\.

------
#### [ AWS CLI, AWS API ]

**To delete an AI services opt\-out policy**  
You can use one of the following to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)
+ AWS API: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)

------