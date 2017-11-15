# Creating and Managing an AWS Organization<a name="orgs_manage_org"></a>

You can perform the following tasks using the AWS Organizations console:

+ **Create an organization**\. Create your organization with your current account as its master account\. Create member accounts within your organization, and invite other accounts to join your organization\.

+ **Enable all features in your organization**\. If your organization has only the consolidated billing feature set enabled then you must enable all features to use the advanced features available in AWS Organizations, such as policies that enable you to apply fine\-grained control over which services and actions that member accounts can access\.
**Note**  
If you originally had a Consolidated Billing family of accounts, it was migrated to an organization with only the consolidated billing features enabled\. You can also choose to create your organization with only consolidated billing features enabled\. 

+ **View details about your organization**\. View details about your organization and its roots, organizational units \(OUs\), and accounts\.

+ **Delete an organization**\. Delete an organization when you no longer need it\.

**Note**  
The procedures in this section specify the minimum permissions needed to perform the tasks\. These typically apply to the API or access to the command line tool\.  
Performing a task in the console might require additional permissions\. For example, you could grant read\-only permissions to all users in your organization, and then grant other permissions that allow select users to perform specific tasks\. For an example of a policy that grants read\-only permissions to an organization, see [Granting Limited Access by Actions](orgs_permissions_iam-policies.md#orgs_permissions_grant-limited-actions)\.