# Enabling and disabling SCPs<a name="enable-scps"></a>

Before you can create and attach service control policies \(SCPs\), you must enable this feature\. Enabling the use of SCPs is a one\-time task on the organization root\. You must be signed in to the organization's master account to enable SCPs\.

To enable SCPs, you need permission to run the following actions:
+ `organizations:EnablePolicyType`
+ `organizations:DescribeOrganization`

**To enable SCPs \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, choose **Root** in the left navigation pane\.

1. In the details pane on the right side, next to **Service control policies**, choose **Enable**\.

1. Under **What is a service control policy?**, choose **Enable service control polices**\. 

**To enable SCPs \(AWS CLI, AWS API\)**  
You can use one of the following to enable SCPs:
+ AWS CLI: [aws organizations enable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-policy-type.html)
+ AWS API: [EnablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnablePolicyType.html)

## Disabling SCPs<a name="disable-scps"></a>

When you disable SCPs, all SCPs are automatically detached from all entities in the organization root\. If you reenable SCPs, all entities in the organization root are initially attached to only the default `FullAWSAccess` SCP\. Any attachments of SCPs to entities from before SCPs were disabled are lost\.

You can only disable SCPs from the organization's master account\.

To disable SCPs, you need permission to run the following actions:
+ `organizations:DescribeOrganization`
+ `organizations:DisablePolicyType`

**To disable SCPs \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, choose **Root** in the left navigation pane\.

1. In the details pane on the right, next to **Service control policies**, choose **Disable**\.

**To disable SCPs \(AWS CLI, AWS API\)**  
You can use one of the following to disable SCPs:
+ AWS CLI: [aws organizations disable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-policy-type.html)
+ AWS API: [DisablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisablePolicyType.html)