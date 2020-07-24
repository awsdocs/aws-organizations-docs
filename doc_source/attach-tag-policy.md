# Attaching tag policies<a name="attach-tag-policy"></a>

You can use tag policies on an entire organization as well as on organizational units \(OUs\) and individual accounts\. 
+ When you attach a tag policy to your *organization root*, the tag policy applies to all of that root's member OUs and accounts\.
+ When you attach a tag policy to an *OU*, that tag policy applies to the accounts that belong to the OU\. Those accounts are also subject to any tag policy attached to the organization root\.
+ When you attach a tag policy to an *account*, that tag policy, applies to the account\. In addition, that account is subject to any tag policy attached to the organization root, *plus* any tag policy attached to an OU that the account belongs to\.

The aggregation of any tag policies the account inherits, plus any tag policy directly attached to the account is the [*effective tag policy*](orgs_manage_policies_tag-policies-effective.md)\. For more information, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

**Important**  
Untagged resources don't appear as noncompliant in results\.

To attach tag policies, you must have permission to run the following action:
+ `organizations:AttachPolicy`

**To attach a tag policy to the organization root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the root, OU, or account you want to attach the tag policy to\.

1. In the details pane on the right, expand the **Tag policies** section to see the list of the currently attached tag policies\.

1. On the list of available tag policies, find the one that you want and choose **Attach**\.

**To attach a tag policy to the organization root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following to attach a tag policy:
+ AWS CLI: [aws organizations attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)
+ AWS API: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

**What to Do Next**  
After you attach a tag policy, you can find out how compliant that account's resources are with that tag policy\. To do this, use the Resource Groups console\. For information, see [Evaluating an Account's Compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html#tag-policy-compliance-account) in the *AWS Resource Groups User Guide*\. 

## Detaching a tag policy<a name="detach-tag-policy"></a>

When signed in to your organization's master account, you can detach a tag policy from the organization root, OU, or account that it is attached to\. After you detach a tag policy from an entity, that policy no longer applies to any account that was affected by the now detached entity\. To detach a policy, complete the following steps\. 

To detach a tag policy from the organization root, OU, or account, you must have permission to run the following action:
+ `organizations:DetachPolicy`

**To detach a tag policy from the organization root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the organization root, OU, or account from which you want to detach the policy\.

1. In the **Details** pane on the right, expand the **Tag policies** section to see the list of the currently attached tag policies\. 

1. Find the tag policy that you want to detach and choose **Detach**\. The list of attached tag policies is updated with the chosen policy removed\. 

**To detach a tag policy from the organization root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following to detach a tag policy:
+ AWS CLI: [aws organizations detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)
+ AWS API: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)