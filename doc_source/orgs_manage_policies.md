# Managing AWS Organizations policies<a name="orgs_manage_policies"></a>

Policies in AWS Organizations enable you to apply additional types of management to the AWS accounts in your organization\. You can use policies when [all features are enabled ](orgs_manage_org_support-all-features.md) in your organization\.

The AWS Organizations console displays the enabled or disabled status for each policy type\. On the **Organize accounts** tab, choose the `Root` in the left navigation pane\. The details pane on the right side of the screen shows all of the available policy types\. The list indicates which are enabled and which are disabled in that organization root\. If the option to **Enable** a type is present, that type is currently disabled\. If the option to **Disable** a type is present, that type is currently enabled\.

## Policy types<a name="orgs-policy-types"></a>

Organizations offers policy types in the following two broad categories:

### Authorization policies<a name="orgs-policy-types-list-authorization"></a>

Authorization policies help you to centrally manage the security of the AWS accounts in your organization\.
+ [**Service control policies \(SCPs\)**](orgs_manage_policies_scp.md) offer central control over the maximum available permissions for all of the accounts in your organization\. 

### Management policies<a name="orgs-policy-types-list-management"></a>

Management policies enable you to centrally configure and manage AWS services and their features\.
+ [**Tag policies**](orgs_manage_policies_tag-policies.md) help you standardize the tags attached to the AWS resources in your organization's accounts\. 

## Using policies in your organization<a name="orgs-policy-using"></a>
+ [Enabling and disabling policy types](orgs_manage_policies_enable-disable.md)
+ [Getting information about your organization's policies](orgs_manage_policies_info-operations.md)
+ [How policy inheritance works](orgs_manage_policies-inheritance.md)
+ [Service control policies](orgs_manage_policies_scp.md)
+ [Tag policies](orgs_manage_policies_tag-policies.md)