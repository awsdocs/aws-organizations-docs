# Creating an organization<a name="orgs_manage_org_create"></a>

You can create an organization that starts with your AWS account as the management account\. When you create an organization, you can choose whether the organization supports all features \(recommended\) or only consolidated billing features\. 

After creating an organization, you can add accounts to your organization in these ways from the management account:
+ [Create other AWS accounts](orgs_manage_accounts_create.md) that are automatically added to your organization as member accounts
+ After verifying your email address, [invite existing AWS accounts](orgs_manage_accounts_invites.md#orgs_manage_accounts_invite-account) to join your organization as member accounts

## Create an organization<a name="create-org"></a>

You can create an organization by using either the AWS Management Console or by using a command from the AWS CLI or one of the SDK APIs\.

**Minimum permissions**  
To create an organization with your current AWS account, you must have the following permissions:  
`organizations:CreateOrganization`
`iam:CreateServiceLinkedRole`   
You can restrict this permission to only the service principal `organizations.amazonaws.com`\. 

------
#### [ AWS Management Console ]

**To create an organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. By default, the organization is created with all features enabled\. However, you can choose either of the following steps:
   + To create an organization with all features enabled, on the introduction page, choose **Create an organization**\.
   + To create an organization with Consolidated Billing features only, on the introduction page and under **Create an organization**, choose **consolidated billing features**, and then in the confirmation dialog box, choose **Create an organization**\.

   If you accidentally choose the wrong option, you can immediately go to the **[Settings](https://console.aws.amazon.com/organizations/v2/home/settings)** page, and then choose **Delete organization** and start over\.

1. The organization is created and the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page appears\. The only account present is your management account, and it's currently stored in the [root organizational unit \(OU\)](orgs_getting-started_concepts.md#root)\.

   If required, Organizations automatically sends a verification email to the address that is associated with your management account\. There might be a delay before you receive the verification email\. Verify your email address within 24 hours\. For more information, see [Email address verification](#about-email-verification)\. You can create accounts to grow your organization without verifying your management account's email address\. However, to invite existing accounts, you must first complete email verification\.
**Note**  
If this account previously verified its email address, then it doesn't happen again when you use the account to create an organization\. 

------
#### [ AWS CLI & AWS SDKs ]

**To create an organization**  
You can use one of the following commands to create an organization:
+ AWS CLI: [create\-organization](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-organization.html)

  The following example creates an organization and makes the currently signed\-in AWS account the management account for the organization\.

  ```
  $ aws organizations create-organization
  {
      "Organization": {
          "Id": "o-aa111bb222",
          "Arn": "arn:aws:organizations::123456789012:organization/o-aa111bb222",
          "FeatureSet": "ALL",
          "MasterAccountArn": "arn:aws:organizations::123456789012:account/o-aa111bb222/123456789012",
          "MasterAccountId": "123456789012",
          "MasterAccountEmail": "admin@example.com",
          "AvailablePolicyTypes": [  ...DEPRECATED - DO NOT USE ... ]
      }
  }
  ```
**Important**  
The `AvailablePolicyTypes` field is deprecated and doesn't contain accurate information about the policies enabled in your organization\. To see the accurate and complete list of policy types that are actually enabled for the organization, use the `ListRoots` command, as described in the AWS CLI portion of the following section\.
+ AWS SDKs: [CreateOrganization](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateOrganization.html)

------

Now you can add additional accounts to your organization as follows:
+ To create an AWS account that automatically becomes part of your AWS organization, see [Creating an AWS account in your organization](orgs_manage_accounts_create.md)\.
+ To invite an existing account to your organization, see [Inviting an AWS account to join your organization](orgs_manage_accounts_invites.md)\. 

## Email address verification<a name="about-email-verification"></a>

After you create an organization and before you can invite accounts to join, you must verify that you own the email address provided for the management account in the organization\. 

When you create an organization, if the management account has not been previously verified, AWS automatically sends a verification email to the specified email address\. There might be a delay before you receive the verification email\. 

Within 24 hours, follow the instructions in the email to verify your email address\. 

If you don't verify your email address within 24 hours, you can resend the verification request so that you can invite other AWS accounts to your organization\. If you don't receive the verification email, check that your email address is correct and, if necessary, modify it\.
+ To find out what email address is associated with your management account, see [Viewing the details of an organization from the management account](orgs_manage_org_details.md#orgs_view_org)\.
+ To change the email address that is associated with your management account, see [Managing an AWS account](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html) in the *AWS Billing User Guide*\.

------
#### [ AWS Management Console ]

**To resend the verification request**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. Navigate to the **[Settings](https://console.aws.amazon.com/organizations/v2/home/settings)** page and then choose **Send verification request**\. The option is only present if the management account is not verified\.

1. Verify your email address within 24 hours\.

   After verifying your email address, you can invite other AWS accounts to your organization\. For more information, see [Inviting an AWS account to join your organization](orgs_manage_accounts_invites.md)\.

------

If you change the email address of the management account, the account's status reverts to "email unverified," and you must complete the verification process for your new email address\. 

**Note**  
If you invited accounts to join your organization before you changed the management account's email address and those invitations have not yet been accepted, they can’t be accepted until you verify the management account’s new email address\. Use the previous procedure to resend the verification request\. After you complete the process by responding to the email, your invited accounts can accept the invitations\.