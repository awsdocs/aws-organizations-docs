# How SCPs work<a name="orgs_manage_policies_scps_about"></a>

The following illustration shows how [SCPs](orgs_manage_policies_scps.md) work\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/How_SCP_Permissions_Work.png)

In this illustration, assume that the oval on the left represents an SCP that is attached to the organization's root\. It allows permissions A, B, and C\. The root contains an organizational unit \(OU\) to which is attached a second SCP represented by the oval on the right\. That second SCP allows permissions C, D, and E\. Because the SCP attached to the root doesn't allow D or E, no OUs or accounts in the organization can use them\. Even though the SCP attached to the OU explicitly allows D and E, they are blocked because they're blocked by the SCP attached to the root\. Because the OU's SCP doesn't allow A or B, those permissions are blocked for the OU and any of its child OUs or accounts\. However, other OUs that might exist under the root can still allow A and B\.

The same example works at any level of the OU structure\. You iterate the process described in the previous paragraph as you traverse each OU in the hierarchy until you get to the child OU or AWS account whose permissions you're interested in\. Moving down the tree to some child OU called X, imagine that the oval on the left represents the inherited, effective permissions permitted by all of the SCPs above OU X in the hierarchy\. The oval on the right represents the SCP attached to an OU or an AWS account contained in OU X\. Again, the intersection of those permissions is what is available to be used the entity on the right\. If that entity is an AWS account, then the intersection is the set of permissions that can be granted to users and roles in that account\. If the entity is an OU, then the intersection is the set of permissions that can be inherited by that OU's children\.

Users and roles in accounts must still be granted permissions using AWS Identity and Access Management \(IAM\) permission policies attached to them or to groups\. The SCPs only determine what permissions are ***available*** to be granted by such policies\. The user can't perform any actions that the applicable SCPs don't allow\. Actions allowed by the SCPs can be used if they are granted to the user or role by one or more IAM permission policies\.

When you attach SCPs to the organization root, OUs, or directly to accounts, all policies that affect a given account are evaluated together using the same rules that govern IAM permission policies:
+ Users and roles in affected accounts can't perform any actions that are listed in the SCP's `Deny` statement\. An explicit `Deny` statement overrides any `Allow` that other SCPs might grant\.
+ Any action that has an explicit `Allow` in an SCP \(such as the default "\*" SCP or by any other SCP that calls out a specific service or action\) can be delegated to users and roles in the affected accounts\.
+ Any action that isn't explicitly allowed by an SCP is implicitly denied and can't be delegated to users or roles in the affected accounts\.

By default, an SCP named `FullAWSAccess` is attached to every organization root, OU, and account\. This default SCP allows all actions and all services\. So in a new organization, until you start creating or manipulating the SCPs, all of your existing IAM permissions continue to operate as they did\. As soon as you apply a new or modified SCP to the organization root or an OU that contains an account, the permissions that your users have in that account become filtered by the SCP\. Permissions that used to work might now be denied if they're not allowed by the SCP at every level of the hierarchy down to the specified account\.

If you disable the SCP policy type on the organization root, all SCPs are automatically detached from all entities in the organization root\. If you reenable SCPs on the organization root, all the original attachments are lost, and all entities are reset to being attached to only the default `FullAWSAccess` SCP\.

For details about the syntax of SCPs, see [SCP syntax](orgs_manage_policies_scps_syntax.md)\.