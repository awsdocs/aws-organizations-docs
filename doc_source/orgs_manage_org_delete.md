# Deleting an Organization<a name="orgs_manage_org_delete"></a>

When you no longer need your organization, you can delete it\. 

**Important**  
If you delete an organization, all history associated with that organization, such as invitations history, is lost and cannot be recovered\. Recreating an organization with the same master account does not recover that information\.
You can only delete an organization after you remove all member accounts from the organization\. If you created some of your member accounts using Organizations, you might be blocked from removing those accounts\. You can remove an account only if it has all of the information required to operate as a stand\-alone AWS account\. AWS Organizations does not collect all all of this for a new member account\. For more information about how to provide that information and remove the account, see [To leave an organization when all required account information has *not* yet been provided \(console\)](orgs_manage_accounts_remove.md#leave-without-all-info)\.

**To delete an organization \(Console\)**
**Minimum permissions**  
To delete an organization, when signed in as a user or role in the master account, you must have the following permissions:  
`organizations:DeleteOrganization`
`organizations:DescribeOrganization` \(console only\)

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Before you can delete an organization, you must first remove all accounts from the organization\. For more information, see [Removing a Member Account from Your Organization](orgs_manage_accounts_remove.md) and [To leave an organization when all required account information has *not* yet been provided \(console\)](orgs_manage_accounts_remove.md#leave-without-all-info)\.

1. On the **Settings** tab, choose **Delete organization**\.

1. On the **Delete organization** confirmation dialog box, choose **Delete organization**\.

**To delete an organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to delete an organization: 

+ AWS CLI: [aws organizations delete\-organization](http://docs.aws.amazon.com/cli/latest/reference/organizations/delete-organization.html)

+ AWS API: [DeleteOrganization](http://docs.aws.amazon.com/organizations/latest/APIReference/API_DeleteOrganization.html)