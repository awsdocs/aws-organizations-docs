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
+ A visual editor that lets you select options and generates the JSON policy text for you\.
+ A text editor that lets you directly create the JSON policy text yourself\. 

The visual editor makes the process easy, but it limits your flexibility\. It's a great way to create your first policies and get comfortable with using them\. After you understand how they work and have started to be limited by what the visual editor provides, you can add advanced features to your policies by editing the JSON policy text yourself\. The visual editor uses only the [@@assign value\-setting operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators), and it doesn't provide any access to the [child control operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators)\. You can add the child control operators only if you manually edit the JSON policy text\.

**To create a backup policy \(console\)**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. On the **Policies** tab, choose **Backup policies**\.

1. On the **Backup policies** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a ****Policy name**** and an optional **Description** for the policy\.

1. \(Optional\) You can add one or more tags to the policy by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\.

1. You can build the policy using the **Visual editor** as described in this procedure\. You can also enter or paste policy text in the **JSON** tab\. For information about backup policy syntax, see [Backup policy syntax and examples](orgs_manage_policies_backup_syntax.md)\.

   If you choose to use the **Visual editor**, select the backup options appropriate for your scenario\. A backup plan consists of three parts\. For more information about these backup plan elements, see [Creating a backup plan](https://docs.aws.amazon.com/aws-backup/latest/devguide/creating-a-backup-plan.html) and [Assigning resources](https://docs.aws.amazon.com/aws-backup/latest/devguide/assigning-resources.html) in the *AWS Backup Developer Guide*\.

   1. Backup plan details
      + The **Backup plan name** can consist of only alphanumeric, hyphen, and underline characters\.
      + You must select at least one **Backup plan region** from the list\. The plan can back up resources in only the selected AWS Regions\.

   1. One or more backup rules that specify how and when AWS Backup is to operate\. Each backup rule defines the following items:
      +  A schedule that includes the frequency of the backup and the time window in which the backup can occur\.
      + Lifecycle options that specify when the backup transitions to cold storage, and when the backup expires\.
      + The name of the backup vault to use\. The **Backup vault name** can consist of only alphanumeric, hyphen, and underline characters\. The backup vault must exist before the plan can successfully run\. Create the vault using the AWS Backup console or AWS CLI commands\.
      + \(Optional\) One or more **Copy to region** rules to also copy the backup to vaults in other AWS Regions\.
      + One or more tag key and value pairs to attach to the backup recovery points created each time this backup plan runs\.

   1. A resource assignment that specifies the resources that AWS Backup should backup with this plan\.
      + The **Resource assignment name** can consist of only alphanumeric, hyphen, and underline characters\.
      + Specify the **IAM role** for AWS Backup to use to perform the backup by its name\. 

        In the console, you don't specify the entire Amazon Resource Name \(ARN\)\. You must include both the role name and its prefix that specifies the type of role\. The prefixes are typically `role` or `service-role` , and they are separated from the role name by a forward slash \('/'\)\. For example, you might enter `role/MyRoleName` or `service-role/MyManagedRoleName`\. This is converted to a full ARN for you when stored in the underlying JSON\.
**Important**  
The specified IAM role must already exist in the account the policy is applied to\. If it does not, the backup plan might successfully start backup jobs, but those backup jobs will fail\.
      + Specify one or more **Resource tag key** and **Tag values** pairs\. If there is more than one tag value, separate them with commas\.

1. When you're finished creating your policy, choose **Create policy**\. The policy appears in your list of available backup policies\. 

------
#### [ AWS CLI, AWS API ]

**To create a backup policy**  
You can use one of the following to create a backup policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  For examples that use the AWS CLI to create a backup policy, see [Using backup policies in the AWS CLI](orgs_manage_policies_backup_cli.md)\.
+ AWS API: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

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

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. On the **Policies** tab, choose **Backup policies**\.

1. On the **Backup policies** page, choose the policy that you want to update\.

1. In the details pane on the right, choose **View details**\. 

1. On the page that shows the backup policy, choose **Edit policy**\.

1. Make your changes either by using the visual editor or by editing the JSON\. 

1. When you're finished updating the policy, choose **Save changes**\.

------
#### [ AWS CLI, AWS API ]

**To update a backup policy**  
You can use one of the following to update a backup policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)
+ AWS API: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

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

**To edit the tags attached to a backup policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. On the **Policies** tab, choose **Backup policies**, and then choose the name of the policy with the tags that you want to edit\.

1. In the chosen policy's details pane, choose **EDIT TAGS**\.

1. You can perform any of these actions on this page:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ AWS CLI, AWS API ]

**To edit the tags attached to a backup policy**  
You can use one of the following commands to edit the tags attached to a backup policy:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS API: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

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

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. The policy that you want to delete must first be detached from all roots, OUs, and accounts\. Follow the steps in [Detaching a backup policy](orgs_manage_policies_backup_attach-detach.md#orgs_manage_policies_backup_detach) to detach the policy from all entities in the organization\.

1. On the **Policies** tab, choose **Backup policies**\.

1. From the **Backup policies** page, choose the policy that you want to delete\. 

1. Choose **Delete policy**\.

------
#### [ AWS CLI, AWS API ]

**To delete a backup policy**  
You can use one of the following commands to delete a backup policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)
+ AWS API: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)

------