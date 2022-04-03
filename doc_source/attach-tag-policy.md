# Attaching and detaching tag policies<a name="attach-tag-policy"></a>

You can use tag policies on an entire organization as well as on organizational units \(OUs\) and individual accounts\. 
+ When you attach a tag policy to your *organization root*, the tag policy applies to all of that root's member OUs and accounts\.
+ When you attach a tag policy to an *OU*, that tag policy applies to the accounts that belong to the OU or any of its child OUs\. Those accounts are also subject to any tag policy attached to the organization root\.
+ When you attach a tag policy to an *account*, that tag policy, applies to the account\. In addition, that account is subject to any tag policy attached to the organization root, *plus* any tag policy attached to an OU that the account belongs to\.

The aggregation of any tag policies the account inherits, plus any tag policy directly attached to the account is the [*effective tag policy*](orgs_manage_policies_tag-policies-effective.md)\. For more information, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

**Important**  
Untagged resources don’t appear as noncompliant in results\.

**Minimum permissions**  
To attach tag policies, you must have permission to run the following action:  
`organizations:AttachPolicy`

------
#### [ AWS Management Console ]

You can attach a tag policy by either navigating to the policy or to the root, OU, or account that you want to attach the policy to\.

**To attach a tag policy by navigating to the root, OU, or account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, navigate to and then choose the name of the root, OU, or account that you want to attach a policy to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. In the **Policies** tab, in the entry for **Tag policies**, choose **Attach**\.

1. Find the policy that you want and choose **Attach policy**\.

   The list of attached tag policies on the **Policies** tab is updated to include the new addition\. The policy change takes effect immediately\.

**To attach a tag policy by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Tag policies](https://console.aws.amazon.com/organizations/v2/home/policies/tag-policy)** page, choose the name of the policy that you want to attach\.

1. On the **Targets** tab, choose **Attach**\.

1. Choose the radio button next to the root, OU, or account that you want to attach the policy to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. Choose **Attach policy**\.

   The list of attached tag policies on the **Targets** tab is updated to include the new addition\. The policy change takes effect immediately\.

------
#### [ AWS CLI & AWS SDKs ]

**To attach a tag policy to the organization root, OU, or account**  
You can use one of the following to attach a tag policy:
+ AWS CLI: [attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)

  The following procedure shows how to attach the tag policy you just created to a single test account\.
  + Attach the tag policy to your test account by running a command like the following:

    ```
    $ aws organizations attach-policy \
        --target-id <account-id> \
        --policy-id p-a1b2c3d4e5
    ```

    This command has no output if it is successful\.
+ AWS SDKs: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

The policy change takes effect immediately\.

------

**What to Do Next**  
After you attach a tag policy, you can find out how compliant your resources are with that tag policy\. To do this, use the Resource Groups console\. For information, see [Evaluating an Account's Compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html#tag-policy-compliance-account) in the *AWS Resource Groups User Guide*\. 

## Detaching a tag policy<a name="detach-tag-policy"></a>

When you sign in to your organization's management account, you can detach a tag policy from the organization root, OU, or account that it is attached to\. After you detach a tag policy from an entity, that policy no longer applies to any account that was affected by the now detached entity\. To detach a policy, complete the following steps\. 

**Minimum permissions**  
To detach a tag policy from the organization root, OU, or account, you must have permission to run the following action:  
`organizations:DetachPolicy`

------
#### [ AWS Management Console ]

You can detach a tag policy by either navigating to the policy or to the root, OU, or account that you want to detach the policy from\.

**To detach a tag policy by navigating to the root, OU, or account it's attached to**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page navigate to the Root, OU, or account that you want to detach a policy from\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\. Choose the name of the Root, OU, or account\.

1. On the **Policies** tab, choose the radio button next to the tag policy that you want to detach, and then choose **Detach**\. 

1. In the confirmation dialog box, choose **Detach policy**\.

   The list of attached tag policies is updated\. The policy change takes effect immediately\.

**To detach a tag policy by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Tag policies](https://console.aws.amazon.com/organizations/v2/home/policies/tag-policy)** page, choose the name of the policy that you want to detach from a root, OU, or account\.

1. On the **Targets** tab, choose the radio button next to the root, OU, or account that you want to detach the policy from\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. Choose **Detach**\.

1. In the confirmation dialog box, choose **Detach**\.

   The list of attached tag policies is updated\. The policy change takes effect immediately\.

------
#### [ AWS CLI & AWS SDKs ]

**To detach a tag policy from the organization root, OU, or account**  
You can use one of the following to detach a tag policy:
+ AWS CLI: [detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)
+ AWS SDKs: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)

The policy change takes effect immediately\.

------
