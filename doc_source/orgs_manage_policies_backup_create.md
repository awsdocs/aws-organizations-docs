# Creating, updating, and deleting backup policies<a name="orgs_manage_policies_backup_create"></a>

**In this topic:**
+ After you enable backup policies for your organization, you can [create a policy](#create-backup-policy-procedure)\.
+ When your backup requirements change, you can [update an existing policy](#update-backup-policy-procedure)\.
+ When you no longer need a policy and after you detach it from all organizational units \(OUs\) and accounts, you can [delete it](#delete-backup-policy-procedure)\.

## Creating a backup policy<a name="create-backup-policy-procedure"></a>

**Minimum permissions**  
To create a backup policy, you need permission to run the following action:  
`organizations:CreatePolicy`

------
#### [ AWS Management Console ]

You can create a backup policy in the AWS Management Console in one of two ways:
+ A visual editor that lets you choose options and generates the JSON policy text for you\.
+ A text editor that lets you directly create the JSON policy text yourself\. 

The visual editor makes the process easy, but it limits your flexibility\. It's a great way to create your first policies and get comfortable with using them\. After you understand how they work and have started to be limited by what the visual editor provides, you can add advanced features to your policies by editing the JSON policy text yourself\. The visual editor uses only the [@@assign value\-setting operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators), and it doesn't provide any access to the [child control operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators)\. You can add the child control operators only if you manually edit the JSON policy text\.

**To create a backup policy \(console\)**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Backup\-policies](https://console.aws.amazon.com/organizations/home/policies/backup-policy)** page in the console\.

1. On the **Backup policies** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a ****Policy name**** and an optional **Policy description**\.

1. \(Optional\) You can add one or more tags to the policy by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\.

1. You can build the policy using the **Visual editor** as described in this procedure\. You can also enter or paste policy text in the **JSON** tab\. For information about backup policy syntax, see [Backup policy syntax and examples](orgs_manage_policies_backup_syntax.md)\.

   If you choose to use the **Visual editor**, select the backup options appropriate for your scenario\. A backup plan consists of three parts\. For more information about these backup plan elements, see [Creating a backup plan](https://docs.aws.amazon.com/aws-backup/latest/devguide/creating-a-backup-plan.html) and [Assigning resources](https://docs.aws.amazon.com/aws-backup/latest/devguide/assigning-resources.html) in the *AWS Backup Developer Guide*\.

   1. Backup plan general details
      + The **Backup plan name** can consist of only alphanumeric, hyphen, and underline characters\.
      + You must select at least one **Backup plan region** from the list\. The plan can back up resources in only the selected AWS Regions\.

   1. One or more backup rules that specify how and when AWS Backup is to operate\. Each backup rule defines the following items:
      +  A schedule that includes the frequency of the backup and the time window in which the backup can occur\.
      + The name of the backup vault to use\. The **Backup vault name** can consist of only alphanumeric, hyphen, and underline characters\. The backup vault must exist before the plan can successfully run\. Create the vault using the AWS Backup console or AWS CLI commands\.
      + \(Optional\) One or more **Copy to region** rules to also copy the backup to vaults in other AWS Regions\.
      + One or more tag key and value pairs to attach to the backup recovery points created each time this backup plan runs\.
      + Lifecycle options that specify when the backup transitions to cold storage, and when the backup expires\.

   1. A resource assignment that specifies which resources that AWS Backup should backup with this plan\. The assignment is made by specifying tag pairs that AWS Backup uses to find and match resources
      + The **Resource assignment name** can consist of only alphanumeric, hyphen, and underline characters\.
      + Specify the **IAM role** for AWS Backup to use to perform the backup by its name\. 

        In the console, you don't specify the entire Amazon Resource Name \(ARN\)\. You must include both the role name and its prefix that specifies the type of role\. The prefixes are typically `role` or `service-role` , and they are separated from the role name by a forward slash \('/'\)\. For example, you might enter `role/MyRoleName` or `service-role/MyManagedRoleName`\. This is converted to a full ARN for you when stored in the underlying JSON\.
**Important**  
The specified IAM role must already exist in the account the policy is applied to\. If it does not, the backup plan might successfully start backup jobs, but those backup jobs will fail\.
      + Specify one or more **Resource tag key** and **Tag values** pairs to identify resources that you want backed up\. If there is more than one tag value, separate the values with commas\.

1. When you're finished creating your policy, choose **Create policy**\. The policy appears in your list of available backup policies\. 

------
#### [ AWS CLI & AWS SDKs ]

**To create a backup policy**  
You can use one of the following to create a backup policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  Create a backup plan as JSON text similar to the following, and store it in a text file\. For complete rules for the syntax, see [Backup policy syntax and examples](orgs_manage_policies_backup_syntax.md)\.

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

  This backup plan specifies that AWS Backup should back up all resources in the affected AWS accounts that are in the specified AWS Regions and that have the tag `dataType` with a value of `PII`\.

  Next, import the JSON policy file backup plan to create a new backup policy in the organization\. Note the policy ID at the end of the policy ARN in the output\.

  ```
  $ aws organizations create-policy \
      --name "MyBackupPolicy" \
      --type BACKUP_POLICY \
      --description "My backup policy" \
      --content file://policy.json{
      "Policy": {
          "PolicySummary": {
              "Arn": "arn:aws:organizations::o-aa111bb222:policy/backup_policy/p-i9j8k7l6m5",
              "Description": "My backup policy",
              "Name": "MyBackupPolicy",
              "Type": "BACKUP_POLICY"
          }
          "Content": "...a condensed version of the JSON policy document you provided in the file...",
      }
  }
  ```
+ AWS SDKs: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

------

**What to do next**  
After you create a backup policy, you can put your policy into effect\. To do that, you can [attach the policy ](attach-tag-policy.md) to the organization root, organizational units \(OUs\), AWS accounts within your organization, or a combination of all of those\. 

## Updating a backup policy<a name="update-backup-policy-procedure"></a>

When signed in to your organization's management account, you can edit a policy that requires changes in your organization\. 

**Minimum permissions**  
To update a backup policy, you must have permission to run the following actions:  
`organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the policy to update \(or "\*"\)
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the policy to update \(or "\*"\)

------
#### [ AWS Management Console ]

**To update a backup policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Backup\-policies](https://console.aws.amazon.com/organizations/home/policies/backup-policy)** page in the console\.

1. On the **Backup policies** page, choose the name of the policy that you want to update\.

1. Choose **Edit policy**\.

1. You can enter a new **Policy name**, **Policy description**\. You can change the policy content by using either the **Visual editor** or by directly editing the **JSON**\. 

1. When you're finished updating the policy, choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To update a backup policy**  
You can use one of the following to update a backup policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)

  The following example renames a backup policy\.

  ```
  $  aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --name "Renamed policy"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/backup_policy/p-i9j8k7l6m5",
              "Name": "Renamed policy",
              "Type": "BACKUP_POLICY",
              "AwsManaged": false
          },
           "Content": "{\"plans\":{\"TestBackupPlan\":{\"regions\":{\"@@assign\":   ....TRUNCATED FOR BREVITY....   "@@assign\":[\"Yes\"]}}}}}}}"
      }
  }
  ```

  The following example adds or changes the description for a backup policy\.

  ```
  $  aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --description "My new description"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/backup_policy/p-i9j8k7l6m5",
              "Name": "Renamed policy",
              "Description": "My new description",
              "Type": "BACKUP_POLICY",
              "AwsManaged": false
          },
         "Content": "{\"plans\":{\"TestBackupPlan\":{\"regions\":{\"@@assign\":   ....TRUNCATED FOR BREVITY....   "@@assign\":[\"Yes\"]}}}}}}}"
      }
  }
  ```

  The following example changes the JSON policy document attached to a backup policy\. In this example, the content is taken from a file called `policy.json` with the following text:

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

  ```
  $ aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --content file://policy.json
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/backup_policy/p-i9j8k7l6m5",
              "Name": "Renamed policy",
              "Description": "My new description",
              "Type": "BACKUP_POLICY",
              "AwsManaged": false
          },
           "Content": "{\"plans\":{\"TestBackupPlan\":{\"regions\":{\"@@assign\":   ....TRUNCATED FOR BREVITY....   "@@assign\":[\"Yes\"]}}}}}}}"
  }
  ```
+ AWS SDKs: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

------

## Editing tags attached to a backup policy<a name="tag-backup-policy-procedure"></a>

When signed in to your organization's management account, you can add or remove the tags attached to a backup policy\. To do this, complete the following steps\.

**Minimum permissions**  
To edit the tags attached to a backup policy in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only – to navigate to the policy\)
`organizations:DescribePolicy` \(console only – to navigate to the policy\)
`organizations:TagResource`
`organizations:UntagResource`

------
#### [ AWS Management Console ]

**To edit the tags attached to an backup policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Backup\-policies](https://console.aws.amazon.com/organizations/home/policies/backup-policy)** page in the console\.

1. Choose the name of the policy with the tags that you want to edit\.

1. On the chosen policy's detail page, choose the **Tags** tab, and then choose **Manage tags**\.

1. You can perform any of these actions on this page:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ AWS CLI & AWS SDKs ]

**To edit the tags attached to a backup policy**  
You can use one of the following commands to edit the tags attached to a backup policy:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------

## Deleting a backup policy<a name="delete-backup-policy-procedure"></a>

When signed in to your organization's management account, you can delete a policy that you no longer need in your organization\. 

Before you can delete a policy, you must first detach it from all attached entities\.

**Minimum permissions**  
To delete a policy, you must have permission to run the following action:  
`organizations:DeletePolicy` with a `Resource` element in the same policy statement that includes the ARN of the policy to delete \(or "\*"\)

------
#### [ AWS Management Console ]

**To delete a backup policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Backup\-policies](https://console.aws.amazon.com/organizations/home/policies/backup-policy)** page in the console\.

1. Choose the policy that you want to delete\. 

1. You must first detach the policy that you want to delete from all roots, OUs, and accounts\. Choose the **Targets** tab, choose the radio button next to each root, OU, or account that is shown in the **Targets** list, and then choose **Detach**\. In the confirmation dialog box, choose **Detach**\.

1. Choose **Delete** at the top of the page\.

1. On the confirmation dialog box, enter the name of the policy, and then choose **Delete**\.

------
#### [ AWS CLI & AWS SDKs ]

**To delete a backup policy**  
You can use one of the following to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)

  The following example deletes the specified policy\. It works only if the policy is not attached to any root, OU, or account\.

  ```
  $ aws organizations delete-policy \
      --policy-id p-i9j8k7l6m5
  ```

  This command produces no output when successful\.
+ AWS SDKs: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)

------