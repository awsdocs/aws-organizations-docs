# Attaching and detaching service control policies<a name="orgs_manage_policies_scps_attach"></a>

**Note**  
AWS Organizations is introducing a new version of the Organizations management console\. You can switch between the old console and the new console by choosing the link in the notice boxes at the top of the console\. We encourage you to try the new version and let us know what you think\. We want your feedback and read each submission\.

When you sign in to your organization's management account, you can attach a service control policy \(SCP\) that you previously created\. You can attach an SCP to the organization root, to an organizational unit \(OU\), or directly to an account\. To attach an SCP, complete the following steps\.

**Minimum permissions**  
To attach an SCP to a root, OU, or account, you need permission to run the following action:  
`organizations:AttachPolicy` with a `Resource` element in the same policy statement that includes "\*" or the Amazon Resource Name \(ARN\) of the specified policy and the ARN of the root, OU, or account that you want to attach the policy to

------
#### [ Old console ]

You can attach an SCP by either navigating to the policy or to the root, OU, or account that you want to attach the policy to\.

**To attach an SCP by navigating to the root, OU, or account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Organize accounts](https://console.aws.amazon.com/organizations/home#/browse)** tab, navigate to and then choose the check box next to the root, OU, or account that you want to attach an SCP to\. You might have to expand OUs \(choose the \+ next to an OU name\) in the navigation pane to find the OU or account that you want\.

1. In the details pane on the right, under **POLICIES**, expand the entry for **Service control policies** by choosing it\.

1. Find the policy that you want and choose **Attach**\.

   A blue box appears around the policy name to indicate that this policy is now attached to the specified entity\.

**To attach an SCP by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/home#/policies/service-control)** page, choose the policy that you want to attach\.

1. In the details pane on the right, expand **Accounts**, **Organizational units**, or **Roots**, depending on what kind of entity you want to attach the policy to\.

1. Next to the account, OU, or root that you want to attach the policy to, choose **Attach**\.

   A blue box appears around the entity to indicate that this entity now has the specified policy attached\. The policy change takes effect immediately, affecting the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU\.

------
#### [ New console ]

You can attach an SCP by either navigating to the policy or to the root, OU, or account that you want to attach the policy to\.

**To attach an SCP by navigating to the root, OU, or account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, navigate to and then choose the check box next to the root, OU, or account that you want to attach an SCP to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. In the **Policies** tab, in the entry for **Service control policies**, choose **Attach**\.

1. Find the policy that you want and choose **Attach policy**\.

   The list of attached SCPs on the **Policies** tab is updated to include the new addition\. The policy change takes effect immediately, affecting the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU\.

**To attach an SCP by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/v2/home/policies/service-control-policy)** page, choose the name of the policy that you want to attach\.

1. On the **Targets** tab, choose **Attach**\.

1. Choose the radio button next to the root, OU, or account that you want to attach the policy to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. Choose **Attach policy**\.

   The list of attached SCPs on the **Targets** tab is updated to include the new addition\. The policy change takes effect immediately, affecting the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU\.

------
#### [ AWS CLI & AWS SDKs ]

**To attach an SCP by navigating to the root, OU, or account**  
You can use one of the following commands to attach an SCP:
+ AWS CLI: [aws organizations attach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html)

  The following example attaches an SCP to an OU\.

  ```
  $ aws organizations attach-policy \
      --policy-id p-i9j8k7l6m5 \
      --target-id ou-a1b2-f6g7h222
  ```

  This command produces no output when successful\.
+ AWS SDKs: [AttachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_AttachPolicy.html)

The policy change takes effect immediately, affecting the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU\.

------

## Detaching an SCP from the organization root, OUs, or accounts<a name="detach_policy"></a>

When you sign in to your organization's management account, you can detach an SCP from the organization root, OU, or account that it is attached to\. After you detach an SCP from an entity, that SCP no longer applies to any account that was affected by the now detached entity\. To detach an SCP, complete the following steps\. 

**Note**  
You can't detach the last SCP from a root, an OU, or an account\. There must be at least one SCP attached to every root, OU, and account at all times\.

**Minimum permissions**  
To detach an SCP from the root, OU, or account, you need permission to run the following action:  
`organizations:DetachPolicy`

------
#### [ Old console ]

You can detach an SCP by either navigating to the policy or to the root, OU, or account that you want to attach the policy from\.

**To detach an SCP by navigating to the root, OU, or account it's attached to**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Organize accounts](https://console.aws.amazon.com/organizations/home#/browse)** tab, navigate to and then choose the check box next to the root, OU, or account that you want to detach an SCP from\. You might have to expand OUs \(choose the \+ next to an OU name\) in the navigation pane to find the OU or account that you want\.

1. In the details pane, under **POLICIES**, expand the entry for **Service control policies** by choosing it\.

1. Find the policy that you want and choose **Detach**\.

   The blue box disappears from around the policy name to indicate that this policy is no longer attached to the specified entity\. The policy change caused by detaching the SCP takes effect immediately\. For example, detaching an SCP immediately affects the permissions of IAM users and roles in the formerly attached account or accounts under the formerly attached organization root or OU\.

**To detach an SCP by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/home#/policies/service-control)** page, choose the name of the policy that you want to detach from a Root, OU, or account\.

1. In the details pane, expand the relevant **Accounts**, **Organizational units**, or **Roots** section, depending on the kind of entity you want to detach the policy from\.

1. Next to the account, OU, or root that you want to attach the policy to, choose **Detach**\.

   The entity disappears from the list to indicate that the policy is no longer attached\. The policy change caused by detaching the SCP takes effect immediately\. For example, detaching an SCP immediately affects the permissions of IAM users and roles in the formerly attached account or accounts under the formerly attached organization root or OU\.

------
#### [ New console ]

You can detach an SCP by either navigating to the policy or to the root, OU, or account that you want to attach the policy from\.

**To detach an SCP by navigating to the root, OU, or account it's attached to**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page navigate to the Root, OU, or account that you want to detach a policy from\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\. Choose the name of the Root, OU, or account\.

1. On the **Policies** tab, choose the radio button next to the SCP that you want to detach, and then choose **Detach**\. 

1. In the confirmation dialog box, choose **Detach policy**\.

   The list of attached SCPs is updated\. The policy change caused by detaching the SCP takes effect immediately\. For example, detaching an SCP immediately affects the permissions of IAM users and roles in the formerly attached account or accounts under the formerly attached organization root or OU\.

**To detach an SCP by navigating to the policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Service control policies](https://console.aws.amazon.com/organizations/v2/home/policies/service-control-policy)** page, choose the name of the policy that you want to detach from a root, OU, or account\.

1. On the **Targets** tab, choose the radio button next to the root, OU, or account that you want to detach the policy from\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the OU or account that you want\.

1. Choose **Detach**\.

1. In the confirmation dialog box, choose **Detach**\.

   The list of attached SCPs is updated\. The policy change caused by detaching the SCP takes effect immediately\. For example, detaching an SCP immediately affects the permissions of IAM users and roles in the formerly attached account or accounts under the formerly attached organization root or OU\.

------
#### [ AWS CLI & AWS SDKs ]

**To detach an SCP from a root, OU, or account**  
You can use one of the following commands to detach an SCP:
+ AWS CLI: [aws organizations detach\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/detach-policy.html)

  The following example detaches the specified SCP from the specified OU\.

  ```
  $ aws organizations detach-policy \
      --policy-id p-i9j8k7l6m5 \
      --target-id ou-a1b2-f6g7h222
  ```
+ AWS SDKs: [DetachPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DetachPolicy.html)

The policy change takes effect immediately, affecting the permissions of IAM users and roles in the attached account or all accounts under the attached root or OU

------