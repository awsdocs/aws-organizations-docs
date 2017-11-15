# Accessing and Administering the Member Accounts in Your Organization<a name="orgs_manage_accounts_access"></a>

When you create an account in your organization, AWS Organizations automatically creates a root user and an IAM role for the account\. However, AWS Organizations doesn't create any IAM users, groups, or other roles\. To access the accounts in your organization, you must use one of the following methods:

+ The account has a root user that you can use to sign in\. We recommend that you use the root user only to create IAM users, groups, and roles, and then always sign in with one of those\. See [Accessing a Member Account as the Root User](#orgs_manage_accounts_access-as-root)\. 

+ If you create an account in your organization, you can access the account by using the preconfigured role that exists in all new accounts that are created this way\. See [Accessing a Member Account That Has a Master Account Access Role](#orgs_manage_accounts_access-cross-account-role)\.

+ If you invite an existing account to join your organization, and the account accepts the invitation, you can then create an IAM role that allows the master account to access the invited account, similar to the role automatically added to an account that is created with AWS Organizations\. To create this role, see [Creating the OrganizationAccountAccessRole in an Invited Member Account](#orgs_manage_accounts_create-cross-account-role)\. After you create the role, you can access it using the steps in [Accessing a Member Account That Has a Master Account Access Role](#orgs_manage_accounts_access-cross-account-role)\.

**Minimum permissions**  
To access an AWS account from any other account in your organization, you must have the following permission:  
`sts:AssumeRole` \- The `Resource` element must be set to either \* or the account ID number of the account with the user who needs to access the new member account\.

## Accessing a Member Account as the Root User<a name="orgs_manage_accounts_access-as-root"></a>

When you create a new account, Organizations initially assigns a long, complex, random password to the root user\. You cannot retrieve this initial password\. To access the account as the root user for the first time, you must go through the account password recovery process\.

**Notes**  
We recommend as a [best practice](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users) that you do not use the root user to access your account for anything other than to create other users and roles with more limited permissions\. Then sign in as one of those users or roles\.
We also recommend that you set [multi\-factor authentication](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) on the root user\. Reset the password, and then [assign an MFA device to the root user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable.html)\.

**To request a new password for the member account's root user \(Console\)**

1. Go to the sign\-in page of the AWS console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\. If you are already signed\-in to AWS, you first have to sign out to see the sign\-in page\.

1. If the **Sign in** page shows three text boxes for **Account ID or alias**, **IAM user name**, and **Password**, then choose **Sign\-in using root account credentials**\.

1. Type the email address associated with your AWS account, and then choose **Next**\. 

1. Choose **Forgot your password?**, and then enter the information required to reset the password to a new one that you provide\. To do this, you must be able to access incoming mail sent to the email address that is associated with the account\.

## Creating the OrganizationAccountAccessRole in an Invited Member Account<a name="orgs_manage_accounts_create-cross-account-role"></a>

By default, if you create a member account as part of your organization, AWS automatically creates a role in the account that grants admin permissions to delegated IAM users in the master account\. By default, that role is named `OrganizationAccountAccessRole`\. For more information, see [Accessing a Member Account That Has a Master Account Access Role](#orgs_manage_accounts_access-cross-account-role)\.

However, member accounts that you *invite* to join your organization ***do not*** automatically get an admin role created\. You have to do this manually, as shown in the following procedure\. This essentially duplicates the role automatically set up for created accounts\. We recommend that you use the same name, `OrganizationAccountAccessRole`, for your manually created roles for consistency and ease of remembering\.

**To create an AWS Organizations admin role in a member account \(Console\)**

1. Sign in to the Identity and Access Management \(IAM\) console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the member account that has permissions to create IAM roles and policies\.

1. In the IAM console, navigate to **Roles**, **Create New Role**\.

1. Specify a role name\. We recommend that you use `OrganizationAccountAccessRole`, which is the default name assigned to the role in new accounts\. Choose **Next Step**\.

1. Choose **Role for Cross\-Account Access**, choose **Select** next to **Provide access between AWS accounts you own**, and then choose **Next Step**\.

1. Type the 12\-digit account ID number of the master account that you want to grant admin access to\. You can optionally choose to require authentication using a multi\-factor authentication \(MFA\) device, if you have MFA enabled and configured\. For more information about MFA, see [Using Multi\-Factor Authentication \(MFA\) in AWS](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.

1. On the **Attach Policy** page, choose the AWS managed policy named `AdministratorAccess`, and then choose **Next Step**\.

1. On the **Review** page, check your settings, paying special note to the link URL that is provided\. Give this URL to users in the member account who need to access the role\. Also make note of the **Role ARN** because you need this in step 11\.

1. Choose **Create Role** to commit your changes\.

1. Sign in to the Identity and Access Management \(IAM\) console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. This time, sign in as a user in the master account who has permissions to create policies and assign the policies to users or groups\.

1. Navigate to **Policies**, **Create Policy**, **Create Your Own Policy**\.
**Note**  
This example shows how to create a policy and attach it to a group\. You can alternatively modify an existing policy\.

1. Type a policy name and description, and then type the following into the **Policy Document** field, replacing the role ARN with the actual ARN that you noted in step 7, using the member account ID number and the role name with the name from step 3\.

   ```
   {
      "Version": "2012-10-17",
      "Statement": {
        "Effect": "Allow",
        "Action": "sts:AssumeRole",
        "Resource": [
          "arn:aws:iam::<memberAccountIdNumber>:role/<roleNameFromStep3>",
          <repeat the line above for each member account, replacing the account ID number and role name>,
          ...
        ]
      }
    }
   ```

1. Choose **Create Policy** to save your changes\.

1. Choose **Groups**, and then choose the name of the group \(not the check box\) that you want to use to delegate administration of the member account\.

1. Choose **Attach Policy**, select the policy that you created in step 11, and then choose **Attach Policy**\.

The users who are members of the selected group now can use the URL that you captured in step 7 to access the role\. They can use it the same way as they would if accessing an account that you create in the organization\. For more information about using the role to administer a member account, see [Accessing a Member Account That Has a Master Account Access Role](#orgs_manage_accounts_access-cross-account-role)\. 

## Accessing a Member Account That Has a Master Account Access Role<a name="orgs_manage_accounts_access-cross-account-role"></a>

When you create a member account using the AWS Organizations console, AWS Organizations *automatically* creates an IAM role in the account\. This role has full administrative permissions in the member account\. The role is also configured to grant that access to the organization's master account\. You can create an identical role for an invited member account by following the steps in [Creating the OrganizationAccountAccessRole in an Invited Member Account](#orgs_manage_accounts_create-cross-account-role)\. To use this role to access the member account, you must sign in as a user from the master account that has permissions to assume the role\. To configure these permissions, perform the following procedure\. We recommend that you grant permissions to groups instead of users for ease of maintenance\.

**To grant permissions to members of an IAM group in the master account to access the role \(Console\)**

1. Sign in to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) as a user with administrator permissions in the master account\. This is required to delegate permissions to the IAM group whose users will access the role in the member account\.

1. In the navigation pane, choose **Groups**, and then choose the name of the group whose members you want to be able to assume the role in the member account\. You can create a new group, if required\.

1. Choose the **Permissions** tab, and then expand the **Inline Policies** section\.

1. If no inline policies exist, choose **click here** to create one\. Otherwise, choose **Create Group Policy**\.

1. Next to **Policy Generator**, choose **Select**\.

1. On the **Edit Permissions** page, set the following options:

   + For **Effect**, choose **Allow**\.

   + For **AWS Service**, choose **AWS Security Token Service**\.

   + For **Actions**, choose **AssumeRole**\.

   + For **Amazon Resource Name \(ARN\)**, type the ARN of the role that was created in the account\. You can see the ARN in the IAM console on the role's **Summary** page\. To construct this ARN, use the following template:

     `arn:aws:iam::accountIdNumber:role/rolename`

     Substitute the account ID number of the member account and the role name that was configured when you created the account\. If you did not specify a role name, then the name defaults to `OrganizationAccountAccessRole`\. The ARN should look like the following:

     `arn:aws:iam::123456789012:role/OrganizationAccountAccessRole`

1. Choose **Add statement**, and then choose **Next step**\.

1. On the **Review Policy** page, ensure that the ARN for the role is correct\. Type a name for the new policy, and then choose **Apply Policy**\.

   IAM users that are members of the group now have permissions to switch to the new role in the AWS Management Console\. When using the role, the user has administrator permissions in the new member account\.

1. Provide the information to the user who will switch to the role in the console\. The user needs the account number and the role name to enter manually in the AWS Management Console, or you can send the user a link that is constructed as shown in the following example\. It is shown on multiple lines here for readability, but you should type it and provide it all as one line:

   ```
   https://signin.aws.amazon.com/switchrole
                   ?account=accountIdNumber
                   &roleName=roleName
                   &displayName=textToDisplay
   ```

   The *textToDisplay* is a string that is displayed on the navigation bar in place of the user name\.

For more information about granting permissions to switch roles, see [Granting a User Permissions to Switch Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_permissions-to-switch.html) in the *IAM User Guide*\.

**To manually switch to the role for the member account \(Console\)**

If you provide your users with a link formatted as shown in the preceding procedure, then they simply have to click the link\. They do not have to follow this procedure\.

1. Sign\-in to the AWS Management Console as the master account user who was granted permissions in the preceding procedure\. For example, you can use the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the upper\-right corner, choose the link that contains your current sign\-in name, and then choose **Switch role**\.

1. Type the account ID number and role name provided by your administrator\.

1. For **Display Name**, type the text that you want to show on the navigation bar in the upper\-right corner in place of your user name while you are using the role\. You can optionally select a color\.

1. Choose **Switch Role**\. Now, all actions that you perform are done with the permissions granted to the role that you switched to\. You no longer have the permissions associated with your original IAM user until you switch back\.

1. When you are done performing actions that require the permissions of the role, you can switch back to your normal IAM user by choosing the role name in the upper\-right corner \(whatever you specified as the **Display Name**\), and then choosing **Back to *UserName***\.

For more information about using a role that you have been granted permissions to assume, see [Switching to a Role \(AWS Management Console\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html) in the *IAM User Guide*\.

For an end\-to\-end tutorial about using roles for cross\-account access, see [Tutorial: Delegate Access Across AWS Accounts Using IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) in the *IAM User Guide*\.