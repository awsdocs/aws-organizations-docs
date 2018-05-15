# Creating an Organization<a name="orgs_manage_create"></a>

You can use AWS Organizations to create your own organization to consolidate and manage your AWS accounts\. 

You can create an organization that starts with your AWS account as the master account\. After you sign in to your account, you can create other AWS accounts that automatically are added to your organization as member accounts\. You also can invite existing AWS accounts to join as member accounts\. When you create an organization, you can choose whether the organization supports only consolidated billing features or all features\.

**Minimum permissions**  
To create an organization with your current AWS account, you must have the following permissions:  
`organizations:CreateOrganization`

**To create an organization \(Console\)**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the account that you want to be the organization's master account\.

1. On the introduction page, choose **Create organization**\.

1. Choose whether to create the organization with only [ consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) or with [all features](orgs_getting-started_concepts.md#feature-set-all) enabled\.

1. In the **Create organization** confirmation dialog box, choose **Create organization**\.

1. Choose the **Accounts** tab\. The star next to the account email indicates that it is the master account\.

1. From here you can do the following:
   + Follow the steps in [Inviting an AWS Account to Join Your Organization](orgs_manage_accounts_invites.md) to invite other accounts to join your organization\. If the invited account accepts your invitation, the account appears on the AWS Organizations console\.
   + Follow the steps in [Creating an AWS Account in Your Organization](orgs_manage_accounts_create.md) to create an AWS account that automatically is part of your AWS organization\.

**To create an organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to create an organization:
+ AWS CLI: [aws organizations create\-organization](http://docs.aws.amazon.com/cli/latest/reference/organizations/create-organization.html)
+ AWS API: [CreateOrganization](http://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateOrganization.html)