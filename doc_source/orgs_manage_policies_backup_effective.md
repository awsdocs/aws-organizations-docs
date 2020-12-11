# Viewing effective backup policies<a name="orgs_manage_policies_backup_effective"></a>

You can view the effective backup policy for an account from the AWS Management Console, AWS API, or AWS Command Line Interface\. The following section provides a brief overview of the effective backup policy, including an example\.

## What is the effective backup policy?<a name="effective-backup-policy-defined"></a>

The *effective backup policy* specifies the final backup plan settings that apply to an AWS account\. It is the aggregation of any backup policies that the account inherits, plus any backup policy that is directly attached to the account\. When you attach a backup policy to the organization's root, it applies to all accounts in your organization\. When you attach an backup policy to an organizational unit \(OU\), it applies to all accounts and OUs that belong to the OU\. When you attach a policy directly to an account, it applies only to that one AWS account\.

For example, the backup policy attached to the organization root might specify that all accounts in the organization back up all Amazon DynamoDB tables with a default backup frequency of once per week\. A separate backup policy attached directly to one member account with critical information in a table can override the frequency with a value of once per day\. The combination of these backup policies comprises the effective backup policy\. This effective backup policy is determined for each account in the organization individually\. In this example, the result is that all accounts in the organization back up their DynamoDB tables once per week, with the exception of one account that backs up its tables daily\.

For information about how backup policies are combined into the final effective backup policy, see [Policy syntax and inheritance for management policy types](orgs_manage_policies_inheritance_mgmt.md)\.

## Viewing the effective backup policy<a name="how-to-view-effective-backup-policy"></a>

You can view the effective backup policy for an account by using the AWS Management Console, AWS API, or AWS Command Line Interface\.

**Minimum permissions**  
To view the effective backup policy for an account, you must have permission to run the following actions:  
`organizations:DescribeEffectivePolicy`
`organizations:DescribeOrganization` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To view the effective backup policy for an account**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/home/accounts)** page in the console\.

1. Choose the name of the account that you want to attach the policy to\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png) to expand an OU\) to find the account that you want\.

1. Choose the **Policies** tab\.

1. In the **Backup policies** section, and just above the list of **Attached policies**, choose **View the effective backup policy for this AWS account**\.

   The console displays the effective policy applied to the specified account\.
**Note**  
You can't copy and paste an effective policy and use it as the JSON for another backup policy without significant changes\. Backup policy documents must include the [inheritance operators](orgs_manage_policies_inheritance_mgmt.md#policy-operators) that specify how each setting is merged into the final effective policy\. 

------
#### [ AWS CLI & AWS SDKs ]

**To view the effective policy for an account**  
You can use one of the following commands to view the effective backup policy:
+ AWS CLI: [aws organizations describe\-effective\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-effective-policy.html)

  The following example displays the details of a backup policy\.

  ```
  $ aws organizations describe-effective-policy \
  --policy-type BACKUP_POLICY \
  --target-id 123456789012{
      "EffectivePolicy": {
          "LastUpdatedTimestamp": "2020-06-22T14:31:50.748000-07:00",
          "TargetId": "123456789012",
          "PolicyType": "BACKUP_POLICY",
          "PolicyContent": "{\"plans\":{\"pii_backup_plan\":{\"regions\":[\"ap-northeast-2\",\"us-east-1\",\"eu-north-1\"],\
  "selections\":{\"tags\":{\"datatype\":{\"iam_role_arn\":\"arn:aws:iam::$account:role/MyIamRole\",\"tag_value\":[\"PII\"],\
  "tag_key\":\"dataType\"}}},\"rules\":{\"hourly\":{\"complete_backup_window_minutes\":\"10080\",\"target_backup_vault_name\
  ":\"FortKnox\",\"start_backup_window_minutes\":\"480\",\"schedule_expression\":\"cron(0 5/1 ? * * *)\",\"lifecycle\":{\"mo
  ve_to_cold_storage_after_days\":\"180\",\"delete_after_days\":\"270\"},\"copy_actions\":{\"arn:aws:backup:us-east-1:$accou
  nt:backup-vault:secondary-vault\":{\"lifecycle\":{\"move_to_cold_storage_after_days\":\"10\",\"delete_after_days\":\"100\"
  }}}}}}}}"
      }
  }
  ```
+ AWS SDKs: [DescribeEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeEffectivePolicy.html) 

------