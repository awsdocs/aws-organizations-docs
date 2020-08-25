# Inheritance for service control policies<a name="orgs_manage_policies_inheritance_auth"></a>

**Important**  
The information in this section does ***not*** apply to management policy types, such as tag policies\. See the next section [Policy syntax and inheritance for management policy types](orgs_manage_policies_inheritance_mgmt.md)\.

## Service control policies \(SCPs\)<a name="orgs_manage_policies_inheritance_auth_scps"></a>

Inheritance for service control policies behaves like a filter through which permissions flow to all parts of the tree below\. Imagine that the inverted tree structure of the organization is made of branches that connect from the root to all of the OUs and end at the accounts\. All AWS permissions flow into the root of the tree\. Those permissions must then flow past the SCPs attached to the root, OUs, and the account to get to the principal \(an IAM role or user\) making a request\. Each SCP can filter the permissions passing through to the levels below it\. If an action is blocked by a `Deny` statement, then all OUs and accounts affected by that SCP are denied access to that action\. An SCP at a lower level can't add a permission after it is blocked by an SCP at a higher level\. SCPs can ***only*** filter; they never add permissions\.

SCPs do not support inheritance operators that alter how elements of the policy are inherited by child OUs and accounts\.

You can see a list of all policies applied to an account and where that policy comes from\. To do this,choose an account in the AWS Organizations console\. On the account details page, choose **Policies** and then choose **Service Control Policies** in the right\-hand details pane\. The same policy might apply to the account multiple times because the policy can be attached to any or all of the parent containers of the account\. The effective policy that applies to the account is the intersection of allowed permissions of all applicable policies\.

For more information about how to use SCPs, see [Service control policies](orgs_manage_policies_scps.md)\.