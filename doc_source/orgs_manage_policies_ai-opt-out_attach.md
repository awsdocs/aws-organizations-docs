# Attaching and detaching AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_attach"></a>

You can use Artificial Intelligence \(AI\) services opt\-out policies on an entire organization as well as on organizational units \(OUs\) and individual accounts\. What the AI services opt\-out policy applies to depends on what organization element you attach it to: 
+ When you attach an AI services opt\-out policy to your *organization root*, the policy applies to all of that root's member OUs and accounts\.
+ When you attach an AI services opt\-out policy to an *OU*, that policy applies to the accounts that belong to the OU or any of its child OUs\. Those accounts are also subject to any policy attached to the organization root\.
+ When you attach an AI services opt\-out policy to an *account*, that policy applies to only that account\. The account is also subject to any policy attached to the organization root and any OUs that the account belongs to\.

The aggregation of any AI services opt\-out policies the account inherits from the root and parent OUs, as well as any policies directly attached to the account, is the [*effective policy*](orgs_manage_policies_ai-opt-out_effective.md)\. For information about how policies are merged to the effective policy, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

**Minimum permissions**  
To attach AI services opt\-out policies, you must have permission to run the following action:  
`organizations:AttachPolicy`

**To attach an AI services opt\-out policy to the organization root, an OU, or an account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. In the organization's master account, sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the root, OU, or account to which you want to attach the AI services opt\-out policy\.

1. In the right\-hand details pane, expand the **AI services opt\-out policies** section to see the list of the currently attached AI services opt\-out policies\.

1. On the list of available AI services opt\-out policies, find the one that you want and choose **Attach**\.

**To attach an AI services opt\-out policy to the organization root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following to attach an AI services opt\-out policy:
+ AWS CLI: [aws organizations attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)
+ AWS API: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

## Detaching an AI services opt\-out policy<a name="orgs_manage_policies_ai-opt-out_detach"></a>

When signed in to your organization's master account, you can detach an AI services opt\-out policy from the organization root, OU, or account that it is attached to\. After you detach an AI services opt\-out policy from an entity, that policy no longer applies to any account that was previously affected by the now detached entity\. To detach a policy, complete the following steps\. 

**Minimum permissions**  
To detach an AI services opt\-out policy from the organization root, OU, or account, you must have permission to run the following action:  
`organizations:DetachPolicy`

**To detach an AI services opt\-out policy from the organization root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. In the organization's master account, sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the organization root, OU, or account from which you want to detach the policy\.

1. In the **Details** pane on the right, expand the **AI services opt\-out policies** section to see the list of the currently attached AI services opt\-out policies\. 

1. Find the AI services opt\-out policy that you want to detach and choose **Detach**\. The list of attached AI services opt\-out policies is updated with the chosen policy removed\. 

**To detach an AI services opt\-out policy from the organization root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following to detach an AI services opt\-out policy:
+ AWS CLI: [aws organizations detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)
+ AWS API: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)