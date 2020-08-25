# Creating an organization<a name="orgs_manage_create"></a>

Use AWS Organizations to create an organization\.

You can create an organization that starts with your AWS account as the master account\. When you create an organization, you can choose whether the organization supports all features \(recommended\) or only consolidated billing features\. 

**Note**  
Currently, you can have only one root in your organization\.

After creating an organization, you can add accounts to your organization in these ways from the master account:
+ Create other AWS accounts that are automatically added to your organization as member accounts
+ After verifying your email address, invite existing AWS accounts to join your organization as member accounts

**Minimum permissions**  
To create an organization with your current AWS account, you must have the following permissions:  
`organizations:CreateOrganization`
`iam:CreateServiceLinkedRole`   
You can restrict this permission to only the service principal `organizations.amazonaws.com`\. 

**To create an organization \(console\)**

1. Sign in to the AWS Management Console and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the account that you want to be the organization's master account\.

1. On the introduction page, choose **Create organization**\.

1. In the **Create organization** confirmation dialog box, choose **Create organization**\.
**Note**  
By default, the organization is created with all features enabled\. You can also create the organization with only [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) enabled\.

   The organization is created\. You're now on the **Accounts** tab\. The star next to the account email indicates that it's the master account\.

   A verification email is automatically sent to the address that is associated with your master account\. There might be a delay before you receive the verification email\.

1. Verify your email address within 24 hours\. For more information, see [Email address verification](#about-email-verification)\.

1. Add accounts to your organization as follows:
   + To create an AWS account that is automatically part of your AWS organization, see [Creating an AWS account in your organization](orgs_manage_accounts_create.md)\.
   + To invite an existing account to your organization, see [Inviting an AWS account to join your organization](orgs_manage_accounts_invites.md)\. 
**Note**  
You can add new accounts to your organization without verifying your master account's email address\. To invite existing accounts, you must first verify that email address\.

**To create an organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to create an organization:
+ AWS CLI: [aws organizations create\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-organization.html)
+ AWS API: [CreateOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateOrganization.html)

## Email address verification<a name="about-email-verification"></a>

After you create an organization and before you can invite accounts to join, you must verify that you own the email address provided for the master account in the organization\. 

When you create an organization, AWS automatically sends a verification email to the specified email address\. There might be a delay before you receive the verification email\. 

Within 24 hours, follow the instructions in the email to verify your email address\. 

If you don't verify your email address within 24 hours, you can resend the verification request so that you can invite other AWS accounts to your organization\. If you don't receive the verification email, check that your email address is correct and, if necessary, modify it\. 
+ To find out what email address is associated with your master account, see [Viewing details of an organization from the master account](orgs_manage_org_details.md#orgs_view_org)\.
+ To change the email address that is associated with your master account, see [Managing an AWS Account](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html) in the *AWS Billing and Cost Management User Guide*\.

**To resend the verification request**

1. Sign in to the AWS Management Console and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the account that you want to be the organization's master account\.

1. Choose the Settings tab and then choose ** Send verification request**\. 

1. Verify your email address within 24 hours\.

   After verifying your email address, you can invite other AWS accounts to your organization\. For more information, see [Inviting an AWS account to join your organization](orgs_manage_accounts_invites.md)\.

If you change the email address of the master account, the account's status reverts to "email unverified," and you must complete the verification process for your new email address\.