# Viewing effective backup policies<a name="orgs_manage_policies_backup_effective"></a>

You can view the effective backup policy for an account from the AWS Management Console, AWS API, or AWS Command Line Interface\. The following section provides a brief overview of the effective backup policy, including an example\.

## What is the effective backup policy?<a name="effective-backup-policy-defined"></a>

The *effective backup policy* specifies the final backup plan settings that apply to an account\. It is the aggregation of any backup policies that the account inherits, plus any backup policy that is directly attached to the account\. When you attach an backup policy to the organization root, it applies to all accounts in your organization\. When you attach an backup policy to an organizational unit \(OU\), it applies to all accounts and OUs that belong to the OU\. When you attach a policy directly to an account, it applies only to that one account\.

For example, the backup policy attached to the organization root might specify that all accounts in the organization back up all Amazon DynamoDB tables with a default backup frequency of once per week\. A separate backup policy attached directly to one member account with critical information in a table can override the frequency with a value of once per day\. The combination of these backup policies comprises the effective backup policy\. This effective backup policy is determined for each account in the organization individually\. In this example, the result is that all accounts in the organization back up their DynamoDB tables once per week, with the exception of one account that backs up its tables daily\.

For information about how backup policies are combined into the final effective backup policy, see [Policy syntax and inheritance for management policy types](orgs_manage_policies_inheritance_mgmt.md)\.

## Viewing the effective backup policy<a name="how-to-view-effective-backup-policy"></a>

You can view the effective backup policy for an account by using the AWS Management Console, AWS API, or AWS Command Line Interface\.

**Minimum permissions**  
To view the effective backup policy for an account, you must have permission to run the following actions:  
`organizations:DescribeEffectivePolicy`
`organizations:DescribeOrganization`

**To view the effective backup policy for an account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. In the organization's master account, sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\)\.

1. On the **Accounts** tab, choose the account\.

1. In the details pane on the right, expand the **Backup policies** section\. In it, you can see the list of policies that are directly attached, those available to be attached, and those that are inherited\.

1. Choose **View effective policy**\. A separate tab opens showing the effective policy for the specified account\.
**Note**  
You can't copy and paste an effective policy and use it as the JSON for another backup policy without significant changes\. Backup policy documents must include the [inheritance operators](orgs_manage_policies_inheritance_mgmt.md#policy-operators) that specify how each setting is merged into the final effective policy\. 

**To view the effective policy for an account \(AWS CLI, AWS API\)**  
You can use one of the following commands to view the effective backup policy:
+ AWS CLI: [aws organizations describe\-effective\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-effective-policy.html)

  For the complete procedure for using backup policies in the AWS CLI, see [Using backup policies in the AWS CLI](orgs_manage_policies_backup_cli.md)\.
+ AWS API: [DescribeEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeEffectivePolicy.html) 