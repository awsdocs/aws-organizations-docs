# Tutorial: Creating and configuring an organization<a name="orgs_tutorials_basic"></a>

In this tutorial, you create your organization and configure it with two AWS member accounts\. You create one of the member accounts in your organization, and you invite the other account to join your organization\. Next, you use the [allow list](orgs_getting-started_concepts.md#allowlist) technique to specify that account administrators can delegate only explicitly listed services and actions\. This allows administrators to validate any new service that AWS introduces before they permit its use by anyone else in your company\. That way, if AWS introduces a new service, it remains prohibited until an administrator adds the service to the allow list in the appropriate policy\. The tutorial also shows you how to use a [deny list](orgs_getting-started_concepts.md#denylist) to ensure that no users in a member account can change the configuration for the auditing logs that AWS CloudTrail creates\.

The following illustration shows the main steps of the tutorial\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/tutorialorgs.png)

**[Step 1: Create your organization](#tutorial-orgs-step1)**  
In this step, you create an organization with your current AWS account as the master account\. You also invite one AWS account to join your organization, and you create a second account as a member account\.

**[Step 2: Create the organizational units ](#tutorial-orgs-step2)**  
Next, you create two organizational units \(OUs\) in your new organization and place the member accounts in those OUs\.

**[Step 3: Create the service control policies](#tutorial-orgs-step3)**  
You can apply restrictions to what actions can be delegated to users and roles in the member accounts by using [service control policies \(SCPs\)](orgs_manage_policies_scps.md)\. In this step, you create two SCPs and attach them to the OUs in your organization\.

**[Step 4: Testing your organization's policies](#tutorial-orgs-step4)**  
You can sign in as users from each of the test accounts and see the effects that the SCPs have on the accounts\.

None of the steps in this tutorial incurs costs to your AWS bill\. AWS Organizations is a free service\.

## Prerequisites<a name="tut-basic-prereqs"></a>

This tutorial assumes that you have access to two existing AWS accounts \(you create a third as part of this tutorial\) and that you can sign in to each as an administrator\.

The tutorial refers to the accounts as the following:
+ `111111111111` – The account that you use to create the organization\. This account becomes the master account\. The owner of this account has an email address of `OrgAccount111@example.com`\.
+ `222222222222` – An account that you invite to join the organization as a member account\. The owner of this account has an email address of `member222@example.com`\.
+ `333333333333` – An account that you create as a member of the organization\. The owner of this account has an email address of `member333@example.com`\.

Substitute the values above with the values that are associated with your test accounts\. We recommend that you don't use production accounts for this tutorial\.

## Step 1: Create your organization<a name="tutorial-orgs-step1"></a>

In this step, you sign in to account 111111111111 as an administrator, create an organization with that account as the master account, and invite an existing account, 222222222222, to join as a member account\.

1. Sign in to AWS as an administrator of account 111111111111 and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. On the introduction page, choose **Create organization**\.

1. In the **Create organization** confirmation dialog box, choose **Create organization**\.
**Note**  
By default, the organization is created with all features enabled\. You can also create the organization with only [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) enabled\.

   The organization is created\. You're now on the **Accounts** tab\. The star next to the account email indicates that it's the master account\.

   A verification email is automatically sent to the address that is associated with your master account\. There might be a delay before you receive the verification email\.

1. Verify your email address within 24 hours\. For more information, see [Email address verification](orgs_manage_create.md#about-email-verification)\.

You now have an organization with your account as its only member\. This is the master account of the organization\.

### Invite an existing account to join your organization<a name="tut-basic-invite-existing"></a>

Now that you have an organization, you can begin to populate it with accounts\. In the steps in this section, you invite an existing account to join as a member of your organization\.

**To invite an existing account to join**

1. Open the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. Choose the **Accounts** tab\. The star next to the account name indicates that it is the master account\.

   Now you can invite other accounts to join as member accounts\.

1. On the **Accounts** tab, choose **Add account** and then choose **Invite account**\.

1. In the **Account ID or email** box, enter the email address of the owner of the account that you want to invite, similar to the following: **member222@example\.com**\.

1. Type any text that you want into the **Notes** box\. This text is included in the email that is sent to the owner of the account\.

1. Choose **Invite**\. AWS Organizations sends the invitation to the account owner\.
**Important**  
If you get an error that indicates that you exceeded your account limits for the organization or that you can't add an account because your organization is still initializing, wait until one hour after you created the organization and try again\. If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

1. For the purposes of this tutorial, you now need to accept your own invitation\. Do one of the following to get to the **Invitations** page in the console:
   + Open the email that AWS sent from the master account and choose the link to accept the invitation\. When prompted to sign in, do so as an administrator in the invited member account\. 
   + Open the AWS Organizations console \([https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\) and sign in as an administrator of the member account\. Choose **Invitations**\. The number beside the link indicates how many invitations this account has\.

1. On the **Invitations** page, choose **Accept** and then choose **Confirm**\.

1. Sign out of your member account and sign in again as an administrator in your master account\. 

### Create a member account<a name="tut-basic-create-new"></a>

In the steps in this section, you create an AWS account that is automatically a member of the organization\. We refer to this account in the tutorial as 333333333333\.

**To create a member account**

1. On the AWS Organizations console, on the **Accounts** tab, choose **Add account**\.

1. For **Full name**, enter a name for the account, such as **MainApp Account**\.

1. For **Email**, enter the email address of the individual who is to receive communications on behalf of the account\. This value must be globally unique\. No two accounts can have the same email address\. For example, you might use something like **mainapp@example\.com**\.

1. For **IAM role name**, you can leave this blank to automatically use the default role name of `OrganizationAccountAccessRole`, or you can supply your own name\. This role enables you to access the new member account when signed in as an IAM user in the master account\. For this tutorial, leave it blank to instruct AWS Organizations to create the role with the default name\.

1. Choose **Create**\. You might need to wait a short while and refresh the page to see the new account appear on the **Accounts** tab\.
**Important**  
If you get an error that indicates that you exceeded your account limits for the organization or that you can't add an account because your organization is still initializing, wait until one hour after you created the organization and try again\. If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

## Step 2: Create the organizational units<a name="tutorial-orgs-step2"></a>

In the steps in this section, you create organizational units \(OUs\) and place your member accounts in them\. Your hierarchy looks like the following illustration when you're done\. The master account remains in the root\. One member account is moved to the Production OU, and the other member account is moved to the MainApp OU, which is a child of Production\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/orgs-lab-structure.jpg)

**To create and populate the OUs**

1. On the AWS Organizations console, choose the **Organize Accounts** tab and then choose **\+ New organizational unit**\.

1. For the name of the OU, enter **Production** and then choose **Create organizational unit**\.

1. Choose your new **Production** OU to navigate into it and then choose **\+ New organizational unit**\.

1. For the name of the second OU, enter **MainApp** and then choose **Create organizational unit**\.

   Now you can move your member accounts into these OUs\.

1. In the tree view on the left, choose the **Root**\.

1. Select the first member account, 222222222222, and then choose **Move**\.

1. In the **Move accounts** dialog box, choose **Production** and then choose **Move**\.

1. Select the second member account, 333333333333, and then choose **Move**\.

1. In the **Move accounts** dialog box, choose **Production** to expose **MainApp**\. Choose **MainApp** and then choose **Move**\.

## Step 3: Create the service control policies<a name="tutorial-orgs-step3"></a>

In the steps in this section, you create three [service control policies \(SCPs\)](orgs_manage_policies_scps.md) and attach them to the root and to the OUs to restrict what users in the organization's accounts can do\. The first SCP prevents anyone in any of the member accounts from creating or modifying any AWS CloudTrail logs that you configure\. The master account isn't affected by any SCP, so after you apply the CloudTrail SCP, you must create any logs from the master account\.

**To create the first SCP that blocks CloudTrail configuration actions**

1. Choose the **Policies** tab and then choose **Create policy**\.

1. For **Policy name**, enter **Block CloudTrail Configuration Actions**\.

1. In the **Policy** section on the left, select CloudTrail for the service\. Then choose the following actions: **AddTags**, **CreateTrail**, **DeleteTrail**, **RemoveTags**, **StartLogging**, **StopLogging**, and **UpdateTrail**\.

1. Still in the left pane, choose **Add resource** and specify **CloudTrail** and **All Resources**\. Then choose **Add resource**\.

   The policy statement on the right updates to look similar to the following\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "Stmt1234567890123",
               "Effect": "Deny",
               "Action": [       
                   "cloudtrail:AddTags",    
                   "cloudtrail:CreateTrail",       
                   "cloudtrail:DeleteTrail",       
                   "cloudtrail:RemoveTags",       
                   "cloudtrail:StartLogging",       
                   "cloudtrail:StopLogging",       
                   "cloudtrail:UpdateTrail"
               ],
               "Resource": [
                   "*"
               ]
           }
       ]
   }
   ```

1. Choose **Create policy**\.

The second policy defines an [allow list](orgs_getting-started_concepts.md#allowlist) of all the services and actions that you want to enable for users and roles in the Production OU\. When you're done, users in the Production OU can access ***only*** the listed services and actions\.

**To create the second policy that allows approved services for the production OU**

1. From the list of policies, choose **Create policy**\.

1. For **Policy name**, enter **Allow List for All Approved Services**\.

1. Position your cursor in the right pane of the **Policy** section and paste in a policy like the following\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "Stmt1111111111111",
               "Effect": "Allow",
               "Action": [ 
                   "ec2:*",
                   "elasticloadbalancing:*",
                   "codecommit:*",
                   "cloudtrail:*",
                   "codedeploy:*"
                 ],
               "Resource": [ "*" ]
           }
       ]
   }
   ```

1. Choose **Create policy**\.

The final policy provides a [deny list](orgs_getting-started_concepts.md#denylist) of services that are blocked from use in the MainApp OU\. For this tutorial, you block access to Amazon DynamoDB in any accounts that are in the MainApp OU\.

**To create the third policy that denies access to services that can't be used in the MainApp OU**

1. From the **Policies** tab, choose **Create policy**\.

1. For **Policy name**, enter **Deny List for MainApp Prohibited Services**\.

1. In the **Policy** section on the left, select **Amazon DynamoDB** for the service\. For the action, choose **All actions**\.

1. Still in the left pane, choose **Add resource** and specify **DynamoDB** and **All Resources**\. Then choose **Add resource**\.

   The policy statement on the right updates to look similar to the following\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Deny",
         "Action": [ "dynamodb:*" ],
         "Resource": [ "*" ]
       }
     ]
   }
   ```

1. Choose **Create policy** to save the SCP\.

### Enable the service control policy type in the root<a name="tut-basic-enable-scp"></a>

Before you can attach a policy of any type to a root or to any OU within a root, you must enable the policy type for that root\. Policy types aren't enabled in any root by default\. The steps in this section show you how to enable the service control policy \(SCP\) type for the root in your organization\.

**Note**  
Currently, you can have only one root in your organization\. It's created for you and named **Root** when you create your organization\.

**To enable SCPs for your root**

1. On the **Organize accounts** tab, choose your root\.

1. In the **Details** pane on the right, under **ENABLE/DISABLE POLICY TYPES** and next to **Service control policies**, choose **Enable**\.

### Attach the SCPs to your OUs<a name="tut-basic-attach-scp"></a>

Now that the SCPs exist and are enabled for your root, you can attach them to the root and OUs\.

**To attach the policies to the root and the OUs**

1. Still on the **Organize accounts** tab, in the **Details** pane on the right, under **POLICIES**, choose **SERVICE CONTROL POLICIES**\.

1. Choose **Attach** next to the SCP named `Block CloudTrail Configuration Actions` to prevent anyone from altering the way that you configured CloudTrail\. In this tutorial, you attach it to the root so that it affects all member accounts\. 

   The **Details** pane now shows by highlighting that two SCPs are attached to the root: the one you just created and the default `FullAWSAccess` SCP\. 

1. Choose the **Production** OU \(not the check box\) to navigate to its contents\.

1. Under **POLICIES**, choose **SERVICE CONTROL POLICIES** and then choose **Attach** next to `Allow List for All Approved Services` to enable users or roles in member accounts in the Production OU to access the approved services\.

1. The information pane now shows that two SCPs are attached to the OU: the one that you just attached and the default `FullAWSAccess` SCP\. However, because the `FullAWSAccess` SCP is also an allow list that allows all services and actions, you must detach this SCP to ensure that only your approved services are allowed\.

1. To remove the default policy from the Production OU, next to **FullAWSAccess**, choose **Detach**\. After you remove this default policy, all member accounts under the root immediately lose access to all actions and services that are not on the allow list SCP that you attached in the preceding step\. Any requests to use actions that aren't included in the **Allow List for All Approved Services** SCP are denied\. This is true even if an administrator in an account grants access to another service by attaching an IAM permissions policy to a user in one of the member accounts\.

1. Now you can attach the SCP named `Deny List for MainApp Prohibited services` to prevent anyone in the accounts in the MainApp OU from using any of the restricted services\.

   To do this, choose the **MainApp** OU \(not the check box\) to navigate to its contents\.

1. In the **Details** pane, under **POLICIES**, expand the **Service control policies** section\. In the list of available policies, next to **Deny List for MainApp Prohibited Services**, choose **Attach**\.

## Step 4: Testing your organization's policies<a name="tutorial-orgs-step4"></a>

You now can sign in as a user in any of the member accounts and try to perform various AWS actions:
+ If you sign in as a user in the master account, you can perform any operation that is allowed by your IAM permissions policies\. The SCPs don't affect any user or role in the master account, no matter which root or OU the account is located in\.
+ If you sign in as the root user or an IAM user in account 222222222222, you can perform any actions that are allowed by the allow list\. AWS Organizations denies any attempt to perform an action in any service that isn't in the allow list\. Also, AWS Organizations denies any attempt to perform one of the CloudTrail configuration actions\.
+ If you sign in as a user in account 333333333333, you can perform any actions that are allowed by the allow list and not blocked by the deny list\. AWS Organizations denies any attempt to perform an action that isn't in the allow list policy and any action that is in the deny list policy\. Also, AWS Organizations denies any attempt to perform one of the CloudTrail configuration actions\.