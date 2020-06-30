# Enabling and disabling policy types<a name="orgs_manage_policies_enable-disable"></a>

Before you can create and attach a policy to your organization, you must enable that policy type for use\. Enabling a policy type is a one\-time task on the organization root\. You can enable a policy type from only the organization's master account\.

**Minimum permissions**  
To enable a policy type, you need permission to run the following actions:  
`organizations:EnablePolicyType`
`organizations:DescribeOrganization` \(needed only if you're using the [AWS Organizations console](https://console.aws.amazon.com/organizations/)\)
`organizations:ListRoots` \(needed only if you're using the [AWS Organizations console](https://console.aws.amazon.com/organizations/)\)

**To enable a policy type \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, choose **Root** in the left navigation pane\.

1. In the details pane on the right side, under **ENABLE/DISABLE POLICY TYPES**, and next to the policy type you want to use, choose **Enable**\.

**To enable a policy type \(AWS CLI, AWS API\)**  
You can use one of the following commands to enable a policy type:
+ AWS CLI: [aws organizations enable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-policy-type.html)
+ AWS API: [EnablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnablePolicyType.html)

## Disabling a policy type<a name="disable-policy-type"></a>

If you no longer want to use a certain policy type in your organization, you can disable that type to prevent its accidental use\. You can disable a policy type from only the organization's master account\.

**Important**  
When you disable a policy type, all policies of the specified type are automatically detached from all entities in the organization root\. The policies are ***not*** deleted\.
\(Service control policy type only\) If you re\-enable the SCP policy type later, all entities in the organization root are initially attached to only the default `FullAWSAccess` SCP\. Attachments of SCPs to entities are lost when the SCPs are disabled in the organization\. If you late re\-enable SCPs, you must reattach them to the organization's root, OUs, and accounts, as appropriate\.

**Minimum permissions**  
To disable SCPs, you need permission to run the following actions:  
`organizations:DisablePolicyType`
`organizations:DescribeOrganization` \(needed only if you're using the [AWS Organizations console](https://console.aws.amazon.com/organizations/)\)
`organizations:ListRoots` \(needed only if you're using the [AWS Organizations console](https://console.aws.amazon.com/organizations/)\)

**To disable a policy type \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, choose **Root** in the left navigation pane\.

1. In the details pane on the right side, under **ENABLE/DISABLE POLICY TYPES**, and next to the policy type you want to use, choose **Disable**\.

**To disable a policy type \(AWS CLI, AWS API\)**  
You can use one of the following commands to disable a policy type:
+ AWS CLI: [aws organizations disable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-policy-type.html)
+ AWS API: [DisablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisablePolicyType.html)