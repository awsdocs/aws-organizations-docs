# Using backup policies in the AWS CLI<a name="orgs_manage_policies_backup_cli"></a>

## Enabling backup policies for your organization<a name="backup-policy-enable-cli"></a>

Enabling the use of backup policies is a one\-time task\. You enable backup policies for the organization and also on the organization root, even if you plan to attach backup policies to individual accounts only\. 

**To enable backup policies**

1. Find the ID of the root for your organization so you can specify where to enable the backup policy type\. To find the root ID, run the following from a command prompt\. This example shows sample output\.

   ```
   $ aws organizations list-roots
   {
       "Roots": [
           {
               "Id": "r-examplerootid111",
               "Arn": "arn:aws:organizations::111111111111:root/o-exampleorgid/r-examplerootid111",
               "Name": "Root",
               "PolicyTypes": [
                   {
                       "Type": "SERVICE_CONTROL_POLICY",
                       "Status": "ENABLED"
                   },
                   {
                       "Type": "BACKUP_POLICY",
                       "Status": "ENABLED"
                   }
               ]
           }
       ]
   }
   ```

   In this example, `r-examplerootid111` is the root ID of the organization\. You use the root ID of your own organization in the next step\.

1. Run the following to enable backup policies for the specified root in your organization\.

   ```
   $ aws organizations enable-policy-type \
       --policy-type BACKUP_POLICY \
       --root-id r-examplerootid111
   {
       "Root": {
           "Id": "r-examplerootid111",
           "Arn": "arn:aws:organizations::111111111111:root/o-exampleorgid/r-examplerootid111",
           "Name": "Root",
           "PolicyTypes": [
               {
                   "Type": "SERVICE_CONTROL_POLICY",
                   "Status": "ENABLED"
               },
               {
                   "Type": "TAG_POLICY",
                   "Status": "ENABLED"
               },
               {
                   "Type": "BACKUP_POLICY",
                   "Status": "ENABLED"
               }
           ]
       }
   }
   ```

   This command enables backup policies for the organization with the root ID `r-examplerootid111.` 

## Creating a backup policy<a name="backup-create-first-cli"></a>

After you enable backup policies, you're ready to create your first policy of that type\. 

You can use any basic text editor to create a backup policy\. Use JSON syntax, and save the policy as a file with any name and extension in a location of your choice\. Backup policies can have a maximum of 2,500 characters, including spaces\. 

**To create a backup policy**

1. Create a backup policy like the following, and store it in a text file\.

   ```
   {
       "plans": {
           "PII_Backup_Plan": {
               "regions": { "@@assign": [ "ap-northeast-2", "us-east-1", "eu-north-1" ] },
               "rules": {
                   "Hourly": {
                       "schedule_expression": { "@@assign": "cron(0 5/1 ? * * *)" },
                       "start_backup_window_minutes": { "@@assign": "480" },
                       "complete_backup_window_minutes": { "@@assign": "10080" },
                       "lifecycle": {
                           "move_to_cold_storage_after_days": { "@@assign": "180" },
                           "delete_after_days": { "@@assign": "270" }
                       },
                       "target_backup_vault_name": { "@@assign": "FortKnox" },
                       "copy_actions": {
                           "arn:aws:backup:us-east-1:$account:backup-vault:secondary-vault": {
                               "lifecycle": {
                                   "move_to_cold_storage_after_days": { "@@assign": "10" },
                                   "delete_after_days": { "@@assign": "100" }
                               }
                           }
                       }
                   }
               },
               "selections": {
                   "tags": {
                       "datatype": {
                           "iam_role_arn": { "@@assign": "arn:aws:iam::$account:role/MyIamRole" },
                           "tag_key": { "@@assign": "dataType" },
                           "tag_value": { "@@assign": [ "PII" ] }
                       }
                   }
               }
           }
       }
   }
   ```

   This backup policy specifies that AWS Backup should back up all resources in the affected AWS accounts that are in the specified AWS Regions and that have the tag `dataType` with a value of `PII`\. 

1. Import the JSON policy file to create a new policy object in the organization\. Note the policy ID at the end of the policy Amazon Resource Name \(ARN\) in the output\.

   ```
   $ aws organizations create-policy \
       --name "MyTestPolicy" \
       --type BACKUP_POLICY \
       --description "My test policy" \
       --content file://policy.json
   {
       "Policy": {
           "PolicySummary": {
               "Arn": "arn:aws:organizations::o-exampleorgid:policy/ai_opt_out_policy/p-examplepolicyid123",
               "Description": "My test policy",
               "Name": "MyTestPolicy",
               "Type": "AI_OPT_OUT_POLICY"
           }
           "Content": "...a condensed version of the JSON policy document you provided in the file...",
       }
   }
   ```

## Attach the backup policy to a root, OU, or account<a name="backup-attach-first-cli"></a>

After you create a backup policy, you're ready to attach it to your organization root, an organizational unit \(OU\), or an individual account\. Attaching a backup policy to the organization root affects all of your organization's member accounts\. When you attach a backup policy to an individual account, only that account is subject to the policy\. The account is still subject to any backup policy that is attached to the organization root or to any OUs the account is contained in\.

The following procedure shows how to attach the backup policy you just created to a single test account\.
+ Attach the backup policy to your test account by running a command like the following example\. The policy ID comes from the output in the previous step\.

  ```
  $ aws organizations attach-policy \
      --target-id 123456789012 \
      --policy-id p-examplepolicyid123
  ```

## Determining the effective backup policy for an account<a name="backup-get-effective-cli"></a>

To determine what backup policies apply to an account, run the following command from that account\. From the master account, you can use the `--target-id` parameter to get the effective policy of any member account\. If you are signed in to a member account, you can use the command without the `--target-id` parameter to get the effective policy of the AWS account you're signed in to\.

The output displays the merged combination of all backup policies that apply to the specified AWS account: attached directly to the account, attached to the organization root, or attached to one or more of the OUs the account is in\.

The `PolicyContent` element in the sample output that follows is line wrapped for clarity\. In the actual output, the element is presented as a single line of text\.

```
$ aws organizations describe-effective-policy \
--policy-type BACKUP_POLICY \
--target-id 123456789012
{
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