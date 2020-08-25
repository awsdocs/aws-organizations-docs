# Backup policy syntax and examples<a name="orgs_manage_policies_backup_syntax"></a>

This page describes backup policy syntax and provides examples\.

## Syntax for backup policies<a name="backup-policy-syntax-reference"></a>

A backup policy is a plaintext file that is structured according to the rules of [JSON](http://json.org)\. The syntax for backup policies follows the syntax for all management policy types\. For a complete discussion of that syntax, see [Policy syntax and inheritance for management policy types](http://bisdavid.corp.amazon.com/docs/orgsug/policytype-ml/orgs_manage_policies_inheritance_mgmt.html)\. This topic focuses on applying that general syntax to the specific requirements of the backup policy type\.

The bulk of a backup policy is the backup plan and its rules\. The syntax for the backup plan within an AWS Organizations backup policy is structurally identical to the syntax used by AWS Backup, but the key names are different\. In the descriptions of the policy key names below, each includes the equivalent AWS Backup plan key name\. For more information about AWS Backup plans, see [CreateBackupPlan](https://docs.aws.amazon.com/aws-backup/latest/devguide/API_CreateBackupPlan.html) in the *AWS Backup Developer Guide*\.

To be complete and functional, an [effective backup policy](orgs_manage_policies_backup_effective.md) must include more than just a backup plan with its schedule and rules\. The policy must also identify the AWS Regions and the resources to be backed up, and the AWS Identity and Access Management \(IAM\) role that AWS Backup can use to perform the backup\.

The following functionally complete policy shows the basic backup policy syntax\. If this example was attached directly to an account, AWS Backup would back up all resources for that account in the `us-east-1` and `eu-north-1` Regions that have the tag `dataType` with a value of either `PII` or `RED` \. It backs up those resources daily at 5:00 AM to `My_Backup_Vault` and also stores a copy in `My_Secondary_Vault`\. The vaults must already exist in both of the specified AWS Regions for each AWS account that receives the effective policy\. The backup applies the tag `Owner:Backup` to each recovery point\. 

```
{
    "plans": {
        "PII_Backup_Plan": {
            "rules": {
                "My_Hourly_Rule": {
                    "schedule_expression": {
                        "@@assign": "cron(0 5 ? * * *)"
                    },
                    "start_backup_window_minutes": {
                        "@@assign": "60"
                    },
                    "complete_backup_window_minutes": {
                        "@@assign": "604800"
                    },
                    "target_backup_vault_name": {
                        "@@assign": "My_Backup_Vault"
                    },
                    "recovery_point_tags": {
                        "Owner": {
                            "tag_key": {
                                "@@assign": "Owner"
                            },
                            "tag_value": {
                                "@@assign": "Backup"
                            }
                        }
                    },
                    "lifecycle": {
                        "delete_after_days": {
                            "@@assign": "2"
                        },
                        "move_to_cold_storage_after_days": {
                            "@@assign": "180"
                        }
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-west-2:$account:backup-vault:My_Secondary_Vault": {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-west-2:$account:backup-vault:My_Secondary_Vault"
                            },
                            "lifecycle": {
                                "delete_after_days": {
                                    "@@assign": "28"
                                },
                                "move_to_cold_storage_after_days": {
                                    "@@assign": "180"
                                }
                            }
                        }
                    }
                }
            },
            "regions": {
                "@@append": [
                    "us-east-1",
                    "eu-north-1"
                ]
            },
            "selections": {
                "tags": {
                    "My_Backup_Assignment": {
                        "iam_role_arn": {
                            "@@assign": "arn:aws:iam::$account:role/MyIamRole"
                        },
                        "tag_key": {
                            "@@assign": "dataType"
                        },
                        "tag_value": {
                            "@@assign": [
                                "PII",
                                "RED"
                            ]
                        }
                    }
                }
            },
            "backup_plan_tags": {
                "stage": {
                    "tag_key": {
                        "@@assign": "Stage"
                    },
                    "tag_value": {
                        "@@assign": "Beta"
                    }
                }
            },
        }
    }
}
```

Backup policy syntax includes the following components: 
+ `$account` variables – In certain text strings in the policies, you can use the `$account` variable to represent the current AWS account\. When AWS Backup runs a plan in the effective policy, it automatically replaces this variable with the current AWS account in which the effective policy and its plans are running\. 
**Important**  
You can use the `$account` variable only in policy elements that can include an Amazon Resource Name \(ARN\), such as those that specify the backup vault to store the backup in, or the IAM role with permissions to perform the backup\. 

  For example, the following requires that a vault named `My_Vault` exist in each AWS account that the policy applies to\.

  ```
  arn:aws:backup:us-west-2:$account:vault:My_Vault"
  ```

  We recommend that you use AWS CloudFormation stack sets and its integration with Organizations to automatically create and configure backup vaults and IAM roles for each member account in the organization\. For more information, see [Create a stack set with self\-managed permissions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html#create-stack-set-service-managed-permissions) in the *AWS CloudFormation User Guide*\.
+ Inheritance operators – Backup policies can use both the inheritance [value\-setting operators](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and the [child control operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators)\.
+ `plans`

  At the top level key of the policy is the `plans` key\. A backup policy must always start with this fixed key name at the top of the policy file\. You can have one or more backup plans under this key\.
+ Each plan under the `plans` top level key has a key name that consists of the backup plan name assigned by the user\. In the preceding example, the backup plan name is `PII_Backup_Plan`\. You can have multiple plans in a policy, each with its own rules, Regions, selections, and tags\.

  This backup plan key name in a backup policy maps to the value of the `BackupPlanName` key in an AWS Backup plan\. 

  Each plan contains four elements:
  + `rules` – This key contains a collection of rules\. Each rule translates to a scheduled task, with a start time and window in which to back up the resources identified by the `selections` and `regions` elements in the effective backup policy\.
  + `regions` – This key contains an array list of AWS Regions whose resources can be backed up by this policy\.
  + `selections` – This key contains one or more collections of resources \(within the specified `regions`\) that are backed up by the specified `rules`\. 
  + `backup_plan_tags` – This specifies tags that are attached to the backup plan itself\.
+ `rules`

  The `rules` policy key maps to the `Rules` key in an AWS Backup plan\. You can have one or more rules under the `rules` key\. Each rule becomes a scheduled task to perform a backup of the selected resources\.

  Each rule contains a key whose name is the name of the rule\. In the previous example, the rule name is "My\_Hourly\_Rule"\. The value of the rule key is the following collection of rule elements:
  + `schedule_expression` – This policy key maps to the `ScheduleExpression` key in an AWS Backup plan\.

    Specifies the start time of the backup\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value with a [CRON expression](https://www.wikipedia.org/wiki/Cron#CRON_expression) that specifies when AWS Backup is to initiate a backup job\. The general format of the CRON string is: "cron\( \)"\. Each is a number or wildcard\. For example, `cron(0 5 1,3,5 * * *)` starts the backup at 5 AM every Monday, Wednesday, and Friday\. `cron(0 0/1 ? * * *)` starts the backup every hour on the hour, every day of the week\. 
  + `target_backup_vault_name` – This policy key maps to the `TarbetBackupVaultName` key in an AWS Backup plan\.

    Specifies the name of the backup vault in which to store the backup\. You create the value by using AWS Backup\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value with a vault name\.
**Important**  
The vault must already exist when the backup plan is launched the first time\. We recommend that you use AWS CloudFormation stack sets and its integration with Organizations to automatically create and configure backup vaults and IAM roles for each member account in the organization\. For more information, see [Create a stack set with self\-managed permissions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html#create-stack-set-service-managed-permissions) in the *AWS CloudFormation User Guide*\.
  + `start_backup_window_minutes` – This policy key maps to the `StartWindowMinutes` key in an AWS Backup plan\.

     \(Optional\) Specifies the number of minutes to wait before canceling a job that does not start successfully\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a value with an integer number of minutes\.
  + `complete_backup_window_minutes` – This policy key maps to the `CompletionWindowMinutes` key in an AWS Backup plan\.

    \(Optional\) Specifies the number of minutes after a backup job successfully starts before it must complete or it is cancelled by AWS Backup\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a value with an integer number of minutes\.
  + `lifecycle` – This policy key maps to the `Lifecycle` key in an AWS Backup plan\.

    \(Optional\) Specifies when AWS Backup transitions this backup to cold storage and when it expires\. 
    + `move_to_cold_storage_after_days `– This policy key maps to the `MoveToColdStorageAfterDays` key in an AWS Backup plan\.

      Specifies the number of days after the backup occurs before AWS Backup moves the recovery point to cold storage\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a value with an integer number of days\.
    + `delete_after_days` – This policy key maps to the `DeleteAfterDays` key in an AWS Backup plan\.

      Specifies the number of days after the backup occurs before AWS Backup deletes the recovery point\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a value with an integer number of days\. If you transition a backup to cold storage, it must stay there a minimum of 90 days, so this value must be a minimum of 90 days greater than the `move_to_cold_storage_after_days` value\.
  + `copy_actions` – This policy key maps to the `CopyActions` key in an AWS Backup plan\.

    \(Optional\) Specifies that AWS Backup should copy the backup to one or more additional locations\. Each location is described as follows:
    + A key whose name uniquely identifies this copy action\. At this time, the key name must be the Amazon Resource Name \(ARN\) of the backup vault\. This key contains two entries\.
      + `target_backup_vault_arn` – This policy key maps to the `DestinationBackupVaultArn` key in an AWS Backup plan\.

        \(Optional\) Specifies the vault in which AWS Backup stores an additional copy of the backup\. The value of this key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and the ARN of the vault\. 

        You must use the `$account` variable in the ARN in place of the account ID number\. When AWS Backup runs the backup plan, it automatically replaces the variable with the account ID number of the AWS account in which the policy is running\.
**Important**  
If this key is missing then an all lower\-case version of the ARN in the parent key name is used\. Because ARNs are case sensitive, this might not match and the plan fails\. For this reason, we recommend you always supply this key and value\.
The backup vault must already exist the first time you launch the backup plan\. We recommend that you use AWS CloudFormation stack sets and its integration with Organizations to automatically create and configure backup vaults and IAM roles for each member account in the organization\. For more information, see [Create a stack set with self\-managed permissions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html#create-stack-set-service-managed-permissions) in the *AWS CloudFormation User Guide*\.
      + `lifecycle` – This policy key maps to the `Lifecycle` key under the `CopyAction` key in an AWS Backup plan\.

        \(Optional\) Specifies when AWS Backup transitions this copy of a backup to cold storage and when it expires\. 
        + `move_to_cold_storage_after_days `– This policy key maps to the `MoveToColdStorageAfterDays` key in an AWS Backup plan\.

          Specifies the number of days after the backup occurs before AWS Backup moves the recovery point to cold storage\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a value with an integer number of days\.
        + `delete_after_days` – This policy key maps to the `DeleteAfterDays` key in an AWS Backup plan\.

          Specifies the number of days after the backup occurs before AWS Backup deletes the recovery point\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a value with an integer number of days\. If you transition a backup to cold storage, it must stay there a minimum of 90 days, so this value must be a minimum of 90 days greater than the `move_to_cold_storage_after_days` value\.
  + `recovery_point_tags` – This policy key maps to the `RecoverPointTags` key in an AWS Backup plan\.

    \(Optional\) Specifies tags that AWS Backup attaches to each backup that it creates from this plan\. This key's value contains one or more of the following elements:
    + An identifier for this key name and value pair\. This name for each element under `recovery_point_tags` is the tag key name in all lower case, even if the `tag_key` has a different case treatment\. This identifier is ***not*** case sensitive\. In the previous example, this key pair was identified by the name `Owner`\. Each key pair contains the following elements:
      + `tag_key` – Specifies the tag key name to attach to the backup plan\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value\. The value is case sensitive\. 
      + `tag_value` – Specifies the value that is attached to the backup plan and associated with the `tag_key`\. This key contains any of the [inheritance value operators](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators), and one or more values to replace, append, or remove from the effective policy\. The values are case sensitive\.
+ `regions`

  The `regions` policy key specifies which AWS Regions that AWS Backup looks in to find the resources that match the conditions in the `selections` key\. This key contains any of the [inheritance value operators](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and one or more string values for AWS Region codes, for example: `["us-east-1", "eu-north-1"]`\. 
+ `selections`

  The `selections` policy key specifies the resources that are backed up by the plan rules in this policy\. This key roughly corresponds to the [BackupSelection object in AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/API_BackupSelection.html)\. The resources are specified by a query for matching tag key names and values\. The `selections` key contains one key under it – `tags`\. 
  + `tags` – Specifies the tags that identify the resources, and the IAM role that has permission to both query the resources and back them up\. This key's value contains one or more of the following elements: 
    + An identifier for this tag element\. This identifier under `tags` is the tag key name in all lower case, even if the tag to query has a different case treatment\. This identifier is ***not*** case sensitive\. In the previous example, one element was identified by the name `My_Backup_Assignment`\. Each identifier under `tags` contains the following elements:
      + `iam_role_arn` – Specifies the IAM role that has permission to access the resources identified by the tag query in the AWS Regions specified by the `regions` key\. This value contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value that contains the ARN of the role\. AWS Backup uses this role to query for and discover the resources and to perform the backup\.

        You can use the `$account` variable in the ARN in place of the account ID number\. When the backup plan is run by AWS Backup, it automatically replaces the variable with the actual account ID number of the AWS account in which the policy is running\.
**Important**  
The role must already exist when you launch the backup plan the first time\. We recommend that you use AWS CloudFormation stack sets and its integration with Organizations to automatically create and configure backup vaults and IAM roles for each member account in the organization\. For more information, see [Create a stack set with self\-managed permissions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html#create-stack-set-service-managed-permissions) in the *AWS CloudFormation User Guide*\.
      + `tag_key` – Specifies the tag key name to search for\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value\. The value is case sensitive\. 
      + `tag_value` – Specifies the value that must be associated with a key name that matches `tag_key`\. AWS Backup includes the resource in the backup only if both the `tag_key` and `tag_value` match\. This key contains any of the [inheritance value operators](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and one or more values to replace, append, or remove from the effective policy\. The values are case sensitive\.
+ `backup_plan_tags` – Specifies tags that are attached to the backup plan itself\. This does not impact the tags specified in any rules or selections\.

  \(Optional\) You can attach tags to your backup plans\. This key's value is a collection of elements\. 

  The key name for each element under `backup_plan_tags` is the tag key name in all lower case, even if the tag to query has a different case treatment\. This identifier is ***not*** case sensitive\. The value for each of these entries consists of the following keys:
  + `tag_key` – Specifies the tag key name to attach to the backup plan\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value\. This value is case sensitive\.
  + `tag_value` – Specifies the value that is attached to the backup plan and associated with the `tag_key`\. This key contains the [`@@assign` inheritance value operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) and a string value\. This value is case sensitive\.

## Backup policy examples<a name="backup-policy-examples"></a>

The example backup policies that follow are for information purposes only\. In some of the following examples, the JSON whitespace formatting might be compressed to save space\.

### Example 1: Policy assigned to a parent node<a name="backup-policy-example-1"></a>

The following example shows a backup policy that is assigned to one of the parent nodes of an account\.

**Parent policy** – This policy can be attached to the organization's root, or to any OU that is a parent of all of the intended accounts\.

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
                        "arn:aws:backup:us-east-1:$account:backup-vault:secondary_vault": {                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:backup-vault:secondary_vault"
                            },
                            "lifecycle": {
                                "delete_after_days": { "@@assign": "100" },
                                "move_to_cold_storage_after_days": { "@@assign": "10" }
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
                        "tag_value": { "@@assign": [ "PII", "RED" ] }
                    }
                }
            }
        }
    }
}
```

If no other policies are inherited or attached to the accounts, the effective policy rendered in each applicable AWS account looks like the following example\. The CRON expression causes the backup to run once an hour on the hour\. The account ID 123456789012 will be the actual account ID for each account\.

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": [ "us-east-1", "ap-northeast-3", "eu-north-1" ],
            "rules": {
                "hourly": {
                    "schedule_expression": "cron(0 0/1 ? * * *)",
                    "start_backup_window_minutes": "60",
                    "target_backup_vault_name": "FortKnox",
                    "lifecycle": {
                        "to_delete_after_days": "2",
                        "move_to_cold_storage_after_days": "180"
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:secondary_vault" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:secondary_vault"
                            },
                            "lifecycle": {
                                "to_delete_after_days": "28",
                                "move_to_cold_storage_after_days": "180"
                            }
                        }
                    }
                }
            },
            "selections": {
                "tags": {
                    "datatype": {
                        "iam_role_arn": "arn:aws:iam::123456789012:role/MyIamRole",
                        "tag_key": "dataType",
                        "tag_value": [ "PII", "RED" ]
                    }
                }
            }
        }
    }
}
```

### Example 2: A parent policy is merged with a child policy<a name="backup-policy-example-2"></a>

In the following example, an inherited parent policy and a child policy either inherited or directly attached to an AWS account merge to form the effective policy\. 

**Parent policy** – This policy can be attached to the organization's root or to any parent OU\.

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": { "@@append":[ "us-east-1", "ap-northeast-3", "eu-north-1" ] },
            "rules": {
                "Hourly": {
                    "schedule_expression": { "@@assign": "cron(0 0/1 ? * * *)" },
                    "start_backup_window_minutes": { "@@assign": "60" },
                    "target_backup_vault_name": { "@@assign": "FortKnox" },
                    "lifecycle": {
                        "to_delete_after_days": { "@@assign": "2" },
                        "move_to_cold_storage_after_days": { "@@assign": "180" }
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:secondary_vault" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:secondary_vault"
                            },
                            "lifecycle": {
                                "to_delete_after_days": { "@@assign": "28" },
                                "move_to_cold_storage_after_days": { "@@assign": "180" }
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
                        "tag_value": { "@@assign": [ "PII", "RED" ] }
                    }
                }
            }
        }
    }
}
```

**Child policy** – This policy can be attached directly to the account or to an OU any level below the one the parent policy is attached to\.

```
{
    "plans": {
       "Monthly_Backup_Plan": {
            "regions": {
                "@@append":[ "us-east-1", "eu-central-1" ] },
            "rules": {
                "Monthly": {
                    "schedule_expression": { "@@assign": "cron(0 5 1 * ? *)" },
                    "start_backup_window_minutes": { "@@assign": "480" },
                    "target_backup_vault_name": { "@@assign": "Default" },
                    "lifecycle": {
                        "to_delete_after_days": { "@@assign": "365" },
                        "move_to_cold_storage_after_days": { "@@assign": "30" }
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:Default" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:Default"
                            },
                            "lifecycle": { 
                                "to_delete_after_days": { "@@assign": "365" },
                                "move_to_cold_storage_after_days": { "@@assign": "30" }
                            }
                        }
                    }
                }
            },
            "selections": {
                "tags": {
                    "MonthlyDatatype": {
                        "iam_role_arn": { "@@assign": "arn:aws:iam::$account:role/MyMonthlyBackupIamRole" },
                        "tag_key": { "@@assign": "BackupType" },
                        "tag_value": { "@@assign": [ "MONTHLY", "RED" ] }
                    }
                }
            }
        }
    }
}
```

**Resulting effective policy** – The effective policy applied to the accounts contains two plans, each with its own set of rules and set of resources to apply the rules to\. 

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": [ "us-east-1", "ap-northeast-3", "eu-north-1" ],
            "rules": {
                "hourly": {
                    "schedule_expression": "cron(0 0/1 ? * * *)",
                    "start_backup_window_minutes": "60",
                    "target_backup_vault_name": "FortKnox",
                    "lifecycle": {
                        "to_delete_after_days": "2",
                        "move_to_cold_storage_after_days": "180"
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:secondary_vault" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:secondary_vault"
                            },
                            "lifecycle": {
                                "to_delete_after_days": "28",
                                "move_to_cold_storage_after_days": "180"
                            }
                        }
                    }
                }
            },
            "selections": {
                "tags": {
                    "datatype": {
                        "iam_role_arn": "arn:aws:iam::$account:role/MyIamRole",
                        "tag_key": "dataType",
                        "tag_value": [ "PII", "RED" ]
                    }
                }
            }
        },
        "Monthly_Backup_Plan": {
            "regions": [ "us-east-1", "eu-central-1" ],
            "rules": {
                "monthly": {
                    "schedule_expression": "cron(0 5 1 * ? *)",
                    "start_backup_window_minutes": "480",
                    "target_backup_vault_name": "Default",
                    "lifecycle": {
                        "to_delete_after_days": "365",
                        "move_to_cold_storage_after_days": "30"
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:Default" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:Default"
                            },
                            "lifecycle": {
                                "to_delete_after_days": "365",
                                "move_to_cold_storage_after_days": "30"
                            }
                        }
                    }
                }
            },
            "selections": {
                "tags": {
                    "monthlydatatype": {
                        "iam_role_arn": "arn:aws:iam::123456789012:role/MyMonthlyBackupIamRole",
                        "tag_key": "BackupType",
                        "tag_value": [ "MONTHLY", "RED" ]
                    }
                }
            }
        }
    }
}
```

### Example 3: A parent policy prevents any changes by a child policy<a name="backup-policy-example-3"></a>

In the following example, an inherited parent policy uses the [child control operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators) to enforce all settings and prevents them from being changed or overridden by a child policy\. 

**Parent policy** – This policy can be attached to the organization's root or to any parent OU\. The presence of `"@@operators_allowed_for_child_policies": ["@@none"]` at every node of the policy means that a child policy can't make changes of any kind to the plan\. Nor can a child policy add additional plans to the effective policy\. This policy becomes the effective policy for every OU and account under the OU to which it is attached\.

```
{
    "plans": {
       "@@operators_allowed_for_child_policies": ["@@none"],
       "PII_Backup_Plan": {
           "@@operators_allowed_for_child_policies": ["@@none"],
           "regions": {
               "@@operators_allowed_for_child_policies": ["@@none"],
               "@@append":[ "us-east-1", "ap-northeast-3", "eu-north-1" ]
           },
           "rules": {
               "@@operators_allowed_for_child_policies": ["@@none"],
               "Hourly": {
                   "@@operators_allowed_for_child_policies": ["@@none"],
                   "schedule_expression": {
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "@@assign": "cron(0 0/1 ? * * *)"
                    },
                    "start_backup_window_minutes": {
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "@@assign": "60"
                    },
                    "target_backup_vault_name": {
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "@@assign": "FortKnox"
                    },
                    "lifecycle": {
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "to_delete_after_days": {
                            "@@operators_allowed_for_child_policies": ["@@none"],
                            "@@assign": "2"
                        },
                        "move_to_cold_storage_after_days": {
                            "@@operators_allowed_for_child_policies": ["@@none"],
                            "@@assign": "180"
                        }
                    },
                    "copy_actions": {
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "arn:aws:backup:us-east-1:$account:vault:secondary_vault" : {
                            "@@operators_allowed_for_child_policies": ["@@none"],
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:secondary_vault"
                                "@@operators_allowed_for_child_policies": ["@@none"],
                            },
                            "lifecycle": {
                                "@@operators_allowed_for_child_policies": ["@@none"],
                                "to_delete_after_days": {
                                    "@@operators_allowed_for_child_policies": ["@@none"],
                                    "@@assign": "28"
                                },
                                "move_to_cold_storage_after_days": {
                                    "@@operators_allowed_for_child_policies": ["@@none"],
                                    "@@assign": "180"
                                }
                            }
                        }
                    }
                }
            },
            "selections": {
                "@@operators_allowed_for_child_policies": ["@@none"],
                "tags": {
                    "@@operators_allowed_for_child_policies": ["@@none"],
                    "datatype": {
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "iam_role_arn": {
                            "@@operators_allowed_for_child_policies": ["@@none"],
                            "@@assign": "arn:aws:iam::$account:role/MyIamRole"
                        },
                        "@@operators_allowed_for_child_policies": ["@@none"],
                        "tag_key": {
                            "@@operators_allowed_for_child_policies": ["@@none"],
                            "@@assign": "dataType"
                        },
                        "tag_value": {
                            "@@operators_allowed_for_child_policies": ["@@none"],
                            "@@assign": [ "PII", "RED" ]
                        }
                    }
                }
            }
        }
    }
}
```

**Resulting effective policy** – If any child backup policies exist, they are ignored and the parent policy becomes the effective policy\.

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": [ "us-east-1", "ap-northeast-3", "eu-north-1" ],
            "rules": {
                "hourly": {
                    "schedule_expression": "cron(0 0/1 ? * * *)",
                    "start_backup_window_minutes": "60",
                    "target_backup_vault_name": "FortKnox",
                    "lifecycle": {
                        "to_delete_after_days": "2",
                        "move_to_cold_storage_after_days": "180"
                    },
                    "copy_actions": {
                        "target_backup_vault_arn" : "arn:aws:backup:us-east-1:123456789012:vault:secondary_vault",
                        "lifecycle": {
                            "to_delete_after_days": "28",
                            "move_to_cold_storage_after_days": "180"
                        }
                    }
                }
            },
            "selections": {
                "tags": {
                    "datatype": {
                        "iam_role_arn": "arn:aws:iam::123456789012:role/MyIamRole",
                        "tag_key": "dataType",
                        "tag_value": [ "PII", "RED" ]
                    }
                }
            }
        }
    }
}
```

### Example 4: A parent policy prevents changes to one backup plan by a child policy<a name="backup-policy-example-4"></a>

In the following example, an inherited parent policy uses the [child control operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators) to enforce the settings for a single plan and prevents them from being changed or overridden by a child policy\. The child policy can still add additional plans\.

**Parent policy** – This policy can be attached to the organization's root or to any parent OU\. This example is similar to the previous example with all child inheritance operators blocked, except at the `plans` top level\. The `@@append` setting at that level enables child policies to add other plans to the collection in the effective policy\. Any changes to the inherited plan are still blocked\.

The sections in the plan are truncated for clarity\.

```
{
    "plans": {
        "@@operators_allowed_for_child_policies": ["@@append"],
        "PII_Backup_Plan": {
            "@@operators_allowed_for_child_policies": ["@@none"],
            "regions": { ... },
            "rules": { ... },
            "selections": { ... }
        }
    }
}
```

**Child policy** – This policy can be attached directly to the account or to an OU any level below the one the parent policy is attached to\. This child policy defines a new plan\.

The sections in the plan are truncated for clarity\.

```
{
    "plans": {
        "MonthlyBackupPlan": {
            "regions": { ... },
            "rules": { ... },
            "selections": { … }
        }
    }
}
```

**Resulting effective policy** – The effective policy includes both plans\.

```
{
    "plans": {
        "PII_Backup_Plan": {
            "regions": { ... },
            "rules": { ... },
            "selections": { ... }
        },
        "MonthlyBackupPlan": {
            "regions": { ... },
            "rules": { ... },
            "selections": { … }
        }
    }
}
```

### Example 5: A child policy overrides settings in a parent policy<a name="backup-policy-example-5"></a>

In the following example, a child policy uses [value\-setting operators](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators) to override some of the settings inherited from a parent policy\.

**Parent policy** – This policy can be attached to the organization's root or to any parent OU\. Any of the settings can be overridden by a child policy because the default behavior, in the absence of a [child\-control operator](orgs_manage_policies_inheritance_mgmt.md#child-control-operators) that prevents it, is to allow the child policy to `@@assign`, `@@append`, or `@@remove`\. The parent policy contains all of the required elements for a valid backup plan, so it backs up your resources successfully if it is inherited as is\.

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": {
                "@@append":[ "us-east-1", "ap-northeast-3", "eu-north-1" ]
            },
            "rules": {
                "Hourly": {
                    "schedule_expression": { "@@assign": "cron(0 0/1 ? * * *)" },
                    "start_backup_window_minutes": { "@@assign": "60" },
                    "target_backup_vault_name": { "@@assign": "FortKnox" },
                    "lifecycle": {
                        "to_delete_after_days": { "@@assign": "2" },
                        "move_to_cold_storage_after_days": { "@@assign": "180" }
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:t2" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:t2"
                            },
                            "lifecycle": {
                                "to_delete_after_days": { "@@assign": "28" },
                                "move_to_cold_storage_after_days": { "@@assign": "180" }
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
                        "tag_value": { "@@assign": [ "PII", "RED" ] }
                    }
                }
            }
        }
    }
}
```

**Child policy** – The child policy includes only the settings that need to be different from the inherited parent policy\. There must be an inherited parent policy that provides the other required settings when merged into an effective policy\. Otherwise, the effective backup policy contains a backup plan that is not valid and doesn't back up your resources as expected\.

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": { "@@assign":[ "us-west-2", "eu-central-1" ] },
            "rules": {
                "Hourly": {
                    "schedule_expression": { "@@assign": "cron(0 0/2 ? * * *)" },
                    "start_backup_window_minutes": { "@@assign": "80" },
                    "target_backup_vault_name": { "@@assign": "Default" },
                    "lifecycle": {
                        "to_delete_after_days": { "@@assign": "365" },
                        "move_to_cold_storage_after_days": { "@@assign": "30" }
                    }
                }
            }
        }
    }
}
```

**Resulting effective policy** – The effective policy includes settings from both policies, with the settings provided by the child policy overriding the settings inherited from the parent\. In this example, the following changes occur:
+ The list of Regions is replaced with a completely different list\. If you wanted to add a Region to the inherited list, use `@@append` instead of `@@assign` in the child policy\.
+ AWS Backup performs every other hour instead of hourly\.
+ AWS Backup allows 80 minutes for the backup to start instead of 60 minutes\. 
+ AWS Backup uses the `Default` vault instead of `FortKnox`\.
+ The lifecycle is extended for both the transfer to cold storage and the eventual deletion of the backup\.

```
{
    "plans": {
       "PII_Backup_Plan": {
            "regions": [ "us-west-2", "eu-central-1" ],
            "rules": {
                "hourly": {
                    "schedule_expression": "cron(0 0/2 ? * * *)",
                    "start_backup_window_minutes": "80",
                    "target_backup_vault_name": "Default",
                    "lifecycle": {
                        "to_delete_after_days": "365",
                        "move_to_cold_storage_after_days": "30"
                    },
                    "copy_actions": {
                        "arn:aws:backup:us-east-1:$account:vault:secondary_vault" : {
                            "target_backup_vault_arn" :
                                "@@assign" : "arn:aws:backup:us-east-1:$account:vault:secondary_vault"
                            },
                            "lifecycle": {
                                "to_delete_after_days": "28",
                                "move_to_cold_storage_after_days": "180"
                            }
                        }
                    }
                }
            },
            "selections": {
                "tags": {
                    "datatype": {
                        "iam_role_arn": "arn:aws:iam::$account:role/MyIamRole",
                        "tag_key": "dataType",
                        "tag_value": [ "PII", "RED" ]
                    }
                }
            }
        }
    }
}
```