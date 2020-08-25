# Managing AWS Organizations policies<a name="orgs_manage_policies"></a>

Policies in AWS Organizations enable you to apply additional types of management to the AWS accounts in your organization\. You can use policies when [all features are enabled ](orgs_manage_org_support-all-features.md) in your organization\.

The AWS Organizations console displays the enabled or disabled status for each policy type\. On the **Organize accounts** tab, choose the `Root` in the left navigation pane\. The details pane on the right side of the screen shows all of the available policy types\. The list indicates which are enabled and which are disabled in that organization root\. If the option to **Enable** a type is present, that type is currently disabled\. If the option to **Disable** a type is present, that type is currently enabled\.

## Policy types<a name="orgs-policy-types"></a>

Organizations offers policy types in the following two broad categories:

### Authorization policies<a name="orgs-policy-types-list-authorization"></a>

Authorization policies help you to centrally manage the security of the AWS accounts in your organization\.
+ **[Service control policies \(SCPs\)](orgs_manage_policies_scps.md)** offer central control over the maximum available permissions for all of the accounts in your organization\. 

### Management policies<a name="orgs-policy-types-list-management"></a>

Management policies enable you to centrally configure and manage AWS services and their features\.
+ **[Artificial Intelligence \(AI\) services opt\-out policies](orgs_manage_policies_ai-opt-out.md)** enable you to control data collection for AWS AI services for all of your organization's accounts\.
+ **[Backup policies](orgs_manage_policies_backup.md)** help you centrally manage and apply backup plans to the AWS resources across your organization's accounts\.
+ **[Tag policies](orgs_manage_policies_tag-policies.md)** help you standardize the tags attached to the AWS resources in your organization's accounts\. 

The following table summarizes some of the characteristics of each policy type:


| Policy type | Affects master account | Maximum number you can attach to a root, OU, or account | Maximum size | Supports viewing effective policy for OU or account | 
| --- | --- | --- | --- | --- | 
| SCP | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-no.png) No | 5 | 5120 bytes | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-no.png) No | 
| AI services opt\-out policy | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-yes.png) Yes | 5 | 2500 characters | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-yes.png) Yes | 
| Backup policy | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-yes.png) Yes | 10 | 10,000 characters | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-yes.png) Yes | 
| Tag policy | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-yes.png) Yes | 5 | 2500 characters | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/icon-yes.png) Yes | 

## Using policies in your organization<a name="orgs-policy-using"></a>
+ [Enabling and disabling policy types](orgs_manage_policies_enable-disable.md)
+ [Getting information about your organization's policies](orgs_manage_policies_info-operations.md)
+ [Understanding policy inheritance](orgs_manage_policies_inheritance.md)
+ [Service control policies](orgs_manage_policies_scps.md)
+ [AI services opt\-out policies](orgs_manage_policies_ai-opt-out.md)
+ [Backup policies](orgs_manage_policies_backup.md)
+ [Tag policies](orgs_manage_policies_tag-policies.md)