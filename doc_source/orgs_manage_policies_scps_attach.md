# Attaching SCPs<a name="orgs_manage_policies_scps_attach"></a>

When signed in to your organization's master account, you can attach a service control policy \(SCP\) that you previously created\. You can attach an SCP to the organization root, to an organizational unit \(OU\), or directly to an account\. To attach an SCP, complete the following steps\.

To attach an SCP to a root, OU, or account, you need permission to run the following action:
+ `organizations:AttachPolicy` with a `Resource` element in the same policy statement that includes "\*" or the Amazon Resource Name \(ARN\) of the specified policy and the ARN of the root, OU, or account that you want to attach the policy to

**To attach an SCP to a root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the root, OU, or account you want to attach the SCP to\.

1. In the **Details** pane on the right, expand the **Service control policies** section to see the list of the currently attached SCPs\.

1. On the list of available SCPs, find the one that you want and choose **Attach**\. The list of attached SCPs is updated with the new addition\. The SCP goes into effect immediately\. For example, an SCP immediately affects the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU\.

**To attach an SCP to a root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following commands to attach an SCP:
+ AWS CLI: [aws organizations attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)
+ AWS API: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

## Detaching an SCP from the organization root, OUs, or accounts<a name="detach_policy"></a>

When signed in to your organization's master account, you can detach an SCP from the organization root, OU, or account that it is attached to\. After you detach an SCP from an entity, that SCP no longer applies to any account that was affected by the now detached entity\. To detach an SCP, complete the following steps\. 

**Note**  
You can't detach the last SCP from an entity\. There must be at least one SCP attached to all entities at all times\.

To detach an SCP from the organization root, OU, or account, you need permission to run the following action:
+ `organizations:DetachPolicy`

**To detach an SCP from the organization root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to](orgs_manage_ous.md#navigate_tree) and select the check box for the root, OU, or account from which you want to detach the SCP\.

1. In the **Details** pane on the right, expand the **Service control policies** section to see the list of the currently attached SCPs\. The **Source** field tells you where the SCP comes from\. The SCP can be attached directly to the account or OU, or it could be attached to a parent OU or organization root\.

1. Find the SCP that you want to detach and choose **Detach**\. The list of attached SCPs is updated\. The SCP change caused by detaching the SCP goes into effect immediately\. For example, detaching an SCP immediately affects the permissions of IAM users and roles in the formerly attached account or accounts under the formerly attached organization root or OU\.

**To detach an SCP from a root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following commands to detach an SCP:
+ AWS CLI: [aws organizations detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)
+ AWS API: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)