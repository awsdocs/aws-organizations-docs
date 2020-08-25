# Backup policies<a name="orgs_manage_policies_backup"></a>

For information and procedures common to all policy types, see the following topics:
+ [Enable and disable policy types](orgs_manage_policies_enable-disable.md)
+ [Get details about your policies](orgs_manage_policies_info-operations.md)
+ [Policy syntax and inheritance](orgs_manage_policies_inheritance_auth.md)

[AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/) enables you to create [backup plans](https://docs.aws.amazon.com/aws-backup/latest/devguide/about-backup-plans.html) that define how to back up your AWS resources\. The rules in the plan include a variety of settings, such as the backup frequency, the time window during which the backup occurs, the AWS Region containing the resources to back up and the vault in which to store the backup\. You can then apply a backup plan to groups of AWS resources identified by using tags\. You must also identify an AWS Identity and Access Management \(IAM\) role that grants AWS Backup permission to perform the backup operation on your behalf\.

Backup policies in AWS Organizations combine all of those pieces into [JSON](https://json.org) text documents\. You can attach a backup policy to any of the elements in your organization's structure, such as the root, organizational units \(OUs\), and individual accounts\. Organizations applies inheritance rules to combine the policies in the organization's root, any parent OUs, or attached to the account\. This results in an [effective backup policy](orgs_manage_policies_backup_effective.md) for each account\. This effective policy instructs AWS Backup how to automatically back up your AWS resources\.

Backup policies give you granular control over backing up your resources at whatever level your organization requires\. For example, you can specify in a policy attached to the organization's root that all Amazon DynamoDB tables must be backed up\. That policy can include a default backup frequency\. You can then attach a backup policy to OUs that override the backup frequency according to the requirements of each OU\. For example, the `Developers` OU might specify a backup frequency of once per week, while the `Production` OU specifies once per day\.

You can create partial backup policies that individually include only part of the required information to successfully back up your resources\. You can attach these policies to different parts of the organization tree, such as the root or a parent OU, with the intention of those partial policies being inherited by lower\-level OUs and accounts\. When Organizations combines all of the policies for an account by using inheritance rules, the resulting effective policy must have all the required elements\. Otherwise, AWS Backup considers the policy not valid and does not back up the affected resources\.

**Important**  
AWS Backup can only perform a successful backup when it is invoked by a *complete* effective policy that has all of the required elements\.  
Although a partial policy strategy as described earlier can work, if an effective policy for an account is incomplete, it results in errors or resources that are not successfully backed up\. As an alternate strategy, consider requiring that all backup policies be complete and valid by themselves\. Use default values supplied by policies attached higher in the hierarchy, and override them where needed in child policies by including [inheritance child control operators](orgs_manage_policies_inheritance_mgmt.md#policy-operators)\.

The effective backup plan for each AWS account in the organization appears in the AWS Backup console as an immutable plan for that account\. You can view it, but not change it\.

When AWS Backup begins a backup based on a policy\-created backup plan, you can see the status of the backup job in the AWS Backup console\. A user in a member account can see the status and any errors for the backup jobs in that member account\. If you also enable trusted service access with AWS Backup, a user in the organization's master account can see the status and errors for all backup jobs in the organization\. For more information, see [Enabling cross\-account management](https://docs.aws.amazon.com/aws-backup/latest/devguide/manage-cross-account.html#enable-cross-account) in the *AWS Backup Developer Guide*\.

## Getting started with backup policies<a name="orgs_manage_policies-backup_getting-started"></a>

Follow these steps to get started using backup policies\.

1. [Learn about the permissions you must have to perform backup policy tasks](orgs_manage_policies_backup_prereqs.md)\.

1. [Learn about some best practices we recommend when using backup policies](orgs_manage_policies_backup_best-practices.md)\.

1. [Enable backup policies for your organization](orgs_manage_policies_enable-disable.md)\.

1. [Create a backup policy](orgs_manage_policies_backup_create.md)\.

1. [Attach the backup policy to your organization's root, OU, or account](orgs_manage_policies_backup_attach-detach.md#orgs_manage_policies_backup_attach)\.

1. [View the combined effective backup policy that applies to an account](orgs_manage_policies_backup_effective.md)\.

For all of these steps, you sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

**Other information**
+ [Learn backup policy syntax and see example policies](orgs_manage_policies_backup_syntax.md)
+ [Use the AWS CLI to manage your backup policies](orgs_manage_policies_backup_cli.md)