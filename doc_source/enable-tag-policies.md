# Enabling Tag Policies<a name="enable-tag-policies"></a>

Before you can create and attach tag policies, you must enable this feature\. Enabling the use of tag policies is a one\-time task\. You enable tag policies on the organization root, even if you plan to attach tag policies to individual accounts only\. You must be signed in to the organization's master account to enable tag policies\.

To enable tag policies, you need permissions to run the following actions:
+ `organizations:EnablePolicyType`
+ `organizations:DescribeOrganization`
+ `organizations:ListRoots`

**To enable tag policies \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find **Tag policies** and then choose **Enable access**\. When prompted, confirm the action\.

1. Choose the **Policies** tab, and then choose **Tag policies**\.

1. Under **What is a tag policy?**, choose **Enable tag policies**\.

**To enable tag policies \(AWS CLI, AWS API\)**  
You can use one of the following to enable tag policies:
+ AWS CLI: [aws organizations enable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-policy-type.html)

  For the complete procedure for using tag policies in the AWS CLI, see [Using Tag Policies in the AWS CLI](tag-policy-cli.md)\.
+ AWS API: [EnablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnablePolicyType.html)

**What to Do Next**  
After you enable tag policies, you can [create tag policies](orgs_manage_policies_tag-policies-create.md)\.

## Disabling Tag Policies<a name="disable-tag-policies"></a>

When you disable tag policies, all tag policies are automatically detached, but not deleted, from all entities in the organization root\. 

You can disable tag policies from the organization's master account only\.

To disable tag policies, you should have permissions to run the following actions:
+ `organizations:DescribeOrganization`
+ `organizations:DisablePolicyType`
+ `organizations:ListRoots`

**To disable tag policies \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, choose **Root** in the left navigation pane\.

1. In the details pane on the right side, next to **Tag policies**, choose **Disable**\.

**To disable tag policies \(AWS CLI, AWS API\)**  
You can use one of the following to disable tag policies:
+ AWS CLI: [aws organizations disable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-policy-type.html)
+ AWS API: [DisablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisablePolicyType.html)