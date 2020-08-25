# Creating and managing an organization<a name="orgs_manage_org"></a>

You can perform the following tasks using the AWS Organizations console:
+ **[Create an organization](orgs_manage_create.md)**\. Create your organization with your current account as its master account\. Create member accounts within your organization, and invite other accounts to join your organization\.
+ **[Enable all features in your organization](orgs_manage_org_support-all-features.md)**\. Enabling all features is the preferred way to work with AWS Organizations\. When you create an organization, you have the option to enable all features or a subset of features for consolidating billing\. Enabling all features is the default, and it includes consolidated billing features\. 

  With all features enabled, you can use the advanced account management features available in AWS Organizations such as [service control policies \(SCPs\)](orgs_manage_policies_scps.md)\. SCPs offer central control over the maximum available permissions for all accounts in your organization, enabling you to ensure your accounts stay within your organization’s access control guidelines\.
+ **[View details about your organization](orgs_manage_org_details.md)**\. View details about your organization and its roots, organizational units \(OUs\), and accounts\.
+ **[Delete an organization](orgs_manage_org_delete.md)**\. Delete an organization when you no longer need it\.

**Note**  
The procedures in this section specify the minimum permissions needed to perform the tasks\. These typically apply to the API or access to the command line tool\.  
Performing a task in the console might require additional permissions\. For example, you could grant read\-only permissions to all users in your organization, and then grant other permissions that allow select users to perform specific tasks\. 