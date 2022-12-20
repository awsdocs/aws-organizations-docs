# Attaching and detaching AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_attach"></a>

You can use Artificial Intelligence \(AI\) services opt\-out policies on an entire organization as well as on organizational units \(OUs\) and individual accounts\. What the AI services opt\-out policy applies to depends on what organization element you attach it to: 
+ When you attach an AI services opt\-out policy to your *organization root*, the policy applies to all of that root's member OUs and accounts\.
+ When you attach an AI services opt\-out policy to an *OU*, that policy applies to the accounts that belong to the OU or any of its child OUs\. Those accounts are also subject to any policy attached to the organization root\.
+ When you attach an AI services opt\-out policy to an *account*, that policy applies to only that account\. The account is also subject to any policy attached to the organization root and any OUs that the account belongs to\.

The aggregation of any AI services opt\-out policies the account inherits from the root and parent OUs, as well as any policies directly attached to the account, is the [*effective policy*](orgs_manage_policies_ai-opt-out_effective.md)\. For information about how policies are merged to the effective policy, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

**Minimum permissions**  
To attach AI services opt\-out policies, you must have permission to run the following action:  
`organizations:AttachPolicy`

------
#### [ AWS Management Console ]

You can attach an AI services opt\-out policy by either navigating to the policy or to the root, OU, or account that you want to attach the policy to\.

**To attach an AI services opt\-out policy by navigating to the root, OU, or account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, navigate to and then choose the name of the root, OU, or account that you want to attach a policy to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. In the **Policies** tab, in the entry for **AI service opt\-out policies**, choose **Attach**\.

1. Find the policy that you want and choose **Attach policy**\.

   The list of attached AI services opt\-out policies on the **Policies** tab is updated to include the new addition\. The policy change takes effect immediately\.

**To attach an AI services opt\-out policy by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy)** page, choose the name of the policy that you want to attach\.

1. On the **Targets** tab, choose **Attach**\.

1. Choose the radio button next to the root, OU, or account that you want to attach the policy to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. Choose **Attach policy**\.

   The list of attached AI services opt\-out policies on the **Targets** tab is updated to include the new addition\. The policy change takes effect immediately\.

------
#### [ AWS CLI & AWS SDKs ]

**To attach an AI services opt\-out policy to the organization root, OU, or account**  
You can use one of the following to attach an AI services opt\-out policy:
+ AWS CLI: [attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)

  The following example attaches a policy to an OU\.

  ```
  $ aws organizations attach-policy \
      --target-id ou-a1b2-f6g7h222 \
      --policy-id p-i9j8k7l6m5
  ```

  This command produces no output when successful\.
+ AWS SDKs: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

The policy change takes effect immediately\.

------

## Detaching an AI services opt\-out policy<a name="orgs_manage_policies_ai-opt-out_detach"></a>

When you sign in to your organization's management account, you can detach an AI services opt\-out policy from the organization root, OU, or account that it is attached to\. After you detach an AI services opt\-out policy from an entity, that policy no longer applies to any account that was previously affected by the now detached entity\. To detach a policy, complete the following steps\. 

**Minimum permissions**  
To detach an AI services opt\-out policy from the organization root, OU, or account, you must have permission to run the following action:  
`organizations:DetachPolicy`

------
#### [ AWS Management Console ]

You can detach an AI services opt\-out policy by either navigating to the policy or to the root, OU, or account that you want to detach the policy from\.

**To detach an AI services opt\-out policy by navigating to the root, OU, or account it's attached to**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page navigate to the Root, OU, or account that you want to detach a policy from\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\. Choose the name of the Root, OU, or account\.

1. On the **Policies** tab, choose the radio button next to the AI services opt\-out policy that you want to detach, and then choose **Detach**\. 

1. In the confirmation dialog box, choose **Detach policy**\.

   The list of attached AI services opt\-out policies is updated\. The policy change takes effect immediately\.

**To detach an AI services opt\-out policy by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy)** page, choose the name of the policy that you want to detach from a root, OU, or account\.

1. On the **Targets** tab, choose the radio button next to the root, OU, or account that you want to detach the policy from\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. Choose **Detach**\.

1. In the confirmation dialog box, choose **Detach**\.

   The list of attached AI services opt\-out policies is updated\. The policy change takes effect immediately\.

------
#### [ AWS CLI & AWS SDKs ]

**To detach an AI services opt\-out policy from the organization root, OU, or account**  
You can use one of the following to detach an AI services opt\-out policy:
+ AWS CLI: [detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)

  The following example detaches a policy from an OU\.

  ```
  $ aws organizations detach-policy \
      --target-id ou-a1b2-f6g7h222 \
      --policy-id p-i9j8k7l6m5
  ```

  This command produces no output when successful\.
+ AWS SDKs: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)

The policy change takes effect immediately\.

------