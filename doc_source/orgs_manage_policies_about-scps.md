# How SCPs Work<a name="orgs_manage_policies_about-scps"></a>

The following illustration shows how [SCPs](orgs_manage_policies_scp.md) work\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/How_SCP_Permissions_Work.jpg)

In this illustration, the root has an SCP attached that allows permissions A, B, and C\. An OU in that root has an SCP that allows C, D, and E\. Because the root's OU doesn't allow D or E, nothing in the root or any of its children can use them, including the parent OU\. Even though the parent OU explicitly allows them, they end up blocked because they're blocked by the root\. Also, because the OU's SCP doesn't allow A or B, those permissions are blocked for the parent OU and any of its children\. However, other OUs under the root that are peers to the parent OU could allow A and B\.

Users and roles must still be granted permissions using IAM permission policies attached to them or to groups\. The SCPs filter the permissions granted by such policies, and the user can't perform any actions that the applicable SCPs don't allow\. Actions allowed by the SCPs can be used if they are granted to the user or role by one or more IAM permission policies\.

When you attach SCPs to the root, OUs, or directly to accounts, all policies that affect a given account are evaluated together using the same rules that govern IAM permission policies:
+ Any action that has an explicit `Deny` in an SCP can't be delegated to users or roles in the affected accounts\. An explicit `Deny` statement overrides any `Allow` that other SCPs might grant\.
+ Any action that has an explicit `Allow` in an SCP \(such as the default "\*" SCP or by any other SCP that calls out a specific service or action\) can be delegated to users and roles in the affected accounts\.
+ Any action that isn't explicitly allowed by an SCP is implicitly denied and can't be delegated to users or roles in the affected accounts\.

By default, an SCP named `FullAWSAccess` is attached to every root, OU, and account\. This default SCP allows all actions and all services\. So in a new organization, until you start creating or manipulating the SCPs, all of your existing IAM permissions continue to operate as they did\. As soon as you apply a new or modified SCP to a root or OU that contains an account, the permissions that your users have in that account become filtered by the SCP\. Permissions that used to work might now be denied if they're not allowed by the SCP at every level of the hierarchy down to the specified account\.

If you disable the SCP policy type in a root, all SCPs are automatically detached from all entities in that root\. If you reenable SCPs in that root, all the original attachments are lost, and all entities are reset to being attached to only the default `FullAWSAccess` SCP\.

For details about the syntax of SCPs, see [SCP Syntax](orgs_reference_scp-syntax.md)\.