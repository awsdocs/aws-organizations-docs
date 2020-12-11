# Tutorial: Creating and configuring an organization<a name="orgs_tutorials_basic"></a>

In this tutorial, you create your organization and configure it with two AWS member accounts\. You create one of the member accounts in your organization, and you invite the other account to join your organization\. Next, you use the [allow list](orgs_getting-started_concepts.md#allowlist) technique to specify that account administrators can delegate only explicitly listed services and actions\. This allows administrators to validate any new service that AWS introduces before they permit its use by anyone else in your company\. That way, if AWS introduces a new service, it remains prohibited until an administrator adds the service to the allow list in the appropriate policy\. The tutorial also shows you how to use a [deny list](orgs_getting-started_concepts.md#denylist) to ensure that no users in a member account can change the configuration for the auditing logs that AWS CloudTrail creates\.

The following illustration shows the main steps of the tutorial\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/tutorialorgs.png)

**[Step 1: Create your organization](#tutorial-orgs-step1)**  
In this step, you create an organization with your current AWS account as the management account\. You also invite one AWS account to join your organization, and you create a second account as a member account\.

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
+ `111111111111` – The account that you use to create the organization\. This account becomes the management account\. The owner of this account has an email address of `OrgAccount111@example.com`\.
+ `222222222222` – An account that you invite to join the organization as a member account\. The owner of this account has an email address of `member222@example.com`\.
+ `333333333333` – An account that you create as a member of the organization\. The owner of this account has an email address of `member333@example.com`\.

Substitute the values above with the values that are associated with your test accounts\. We recommend that you don't use production accounts for this tutorial\.

## Step 1: Create your organization<a name="tutorial-orgs-step1"></a>

In this step, you sign in to account 111111111111 as an administrator, create an organization with that account as the management account, and invite an existing account, 222222222222, to join as a member account\.

1. Sign in to AWS as an administrator of account 111111111111 and open the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. On the introduction page, choose **Create an organization**\.

1. In the confirmation dialog box, choose **Create an organization**\.
**Note**  
By default, the organization is created with all features enabled\. You can also create the organization with only [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) enabled\.

   AWS creates the organization and shows you your organization's **Dashboard**\. If you're on a different page then choose **Dashboard** in the navigation pane on the left\.

   A verification email is automatically sent to the address that is associated with your management account\. There might be a delay before you receive the verification email\.

1. Verify your email address within 24 hours\. For more information, see [Email address verification](orgs_manage_org_create.md#about-email-verification)\.

You now have an organization with your account as its only member\. This account is the management account of the organization\.

### Invite an existing account to join your organization<a name="tut-basic-invite-existing"></a>

Now that you have an organization, you can begin to populate it with accounts\. In the steps in this section, you invite an existing account to join as a member of your organization\.

**To invite an existing account to join**

1. In the navigation pane, choose **Accounts**\. The management account is identified by a label\.

   Now you can invite other accounts to join as member accounts\.

1. On the **Accounts** tab, choose **Add AWS account**\.

1. On the **Add an AWS account** page **Invite an exisiting AWS account**\.

1. In the box **Email address or account ID of an AWS account to invite** box, enter the email address of the owner of the account that you want to invite, similar to the following: **member222@example\.com**\. Alternatively, if you know the AWS account ID number, then you can enter it instead\.

1. Type any text that you want into the **Message to include** box\. This text is included in the email that is sent to the owner of the account\.

1. Choose **Send invitation**\. AWS Organizations sends the invitation to the account owner\.
**Important**  
If you get an error that indicates that you exceeded your account limits for the organization or that you can't add an account because your organization is still initializing, wait until one hour after you created the organization and try again\. If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

1. For the purposes of this tutorial, you now need to accept your own invitation\. Do one of the following to get to the **Invitations** page in the console:
   + Open the email that AWS sent from the management account and choose the link to accept the invitation\. When prompted to sign in, do so as an administrator in the invited member account\. 
   + Open the AWS Organizations console \([https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\) and sign in as an administrator of the member account\. In the navigation pane, choose **Invitations**\.

1. On the **Invitations** page, choose **Accept** and then choose **Confirm**\.

1. Sign out of your member account and sign in again as an administrator in your management account\. 

### Create a member account<a name="tut-basic-create-new"></a>

In the steps in this section, you create an AWS account that is automatically a member of the organization\. We refer to this account in the tutorial as 333333333333\.

**To create a member account**

1. On the AWS Organizations console, on the **Accounts** page, choose **Add AWS account**\.

1. On the **Add an AWS account page**, choose **Create an AWS account**\. 

1. For **AWS account name**, enter a name for the account, such as **MainApp Account**\.

1. For **Email address of the account's root user**, enter the email address of the individual who is to receive communications on behalf of the account\. This value must be globally unique\. No two accounts can have the same email address\. For example, you might use something like **mainapp@example\.com**\.

1. For **IAM role name**, you can leave this blank to automatically use the default role name of `OrganizationAccountAccessRole`, or you can supply your own name\. This role enables you to access the new member account when signed in as an IAM user in the management account\. For this tutorial, leave it blank to instruct AWS Organizations to create the role with the default name\.

1. Choose **Create AWS account**\. You might need to wait a short while and refresh the page to see the new account appear on the **Accounts** page\.
**Important**  
If you get an error that indicates that you exceeded your account limits for the organization or that you can't add an account because your organization is still initializing, wait until one hour after you created the organization and try again\. If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

## Step 2: Create the organizational units<a name="tutorial-orgs-step2"></a>

In the steps in this section, you create organizational units \(OUs\) and place your member accounts in them\. When you're done, your hierarchy looks like the following illustration\. The management account remains in the root\. One member account is moved to the Production OU, and the other member account is moved to the MainApp OU, which is a child of Production\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/orgs-lab-structure.jpg)

**To create and populate the OUs**
**Note**  
In the steps that follow, you interact with objects for which you can choose either the name of the object itself, or the radio button next to the object\.  
If you choose the name of the object, you open a new page that displays the objects details\.
If you choose the radio button next to the object, you are identifying that object to be acted upon by another action, such as choosing a menu option\.
The steps that follow have you choose the radio button so that you can then act on the associated object by making menu choices\.

1. On the AWS Organizations console, in the navigation pane, choose the **AWS accounts**\.

1. Ensure that **View AWS accounts only** is turned off\.

1. Select the **Root** container \(the radio button, not its name\)\.

1. Choose **Actions**, and then under **Organizational unit**, choose **Create new**\.

1. On the **Create organizational unit in root** page, for the **Organizational unit name**, enter **Production** and then choose **Create organizational unit**\.

1. Choose your new **Production** OU \(the radio button, not its name\)\.

1. Choose **Actions**, and then under **Organizational unit**, choose **Create new**\.

1. On the **Create organizational unit in Production** page, for the name of the second OU, enter **MainApp** and then choose **Create organizational unit**\.

   Now you can move your member accounts into these OUs\.

1. Return to the **AWS Accounts** page by using the navigation pane, and then expand the tree under your **Production** OU by choosing the triangle next to it\. 

   This displays the **MainApp** OU as a child of **Production**\.

1. Select the first member account, 222222222222 \(the radio button, not its name\), choose **Actions**, and then under **AWS account**, choose **Move**\.

1. On the **Move AWS account '*member\-account\-name*'** page, choose **Production** \(the radio button, not its name\) and then choose **Move AWS account**\.

1. Choose the second member account, 333333333333, \(the radio button, not its name\), choose **Actions**, and then under **AWS account**, choose **Move**\.

1. On the **Move AWS account '*member\-account\-name*'** dialog box,  the triangle next to **Production** to expand that branch and expose **MainApp**\. Choose **MainApp** \(the radio button, not its name\) and then under **AWS account**, choose **Move AWS account**\.

## Step 3: Create the service control policies<a name="tutorial-orgs-step3"></a>

In the steps in this section, you create three [service control policies \(SCPs\)](orgs_manage_policies_scps.md) and attach them to the root and to the OUs to restrict what users in the organization's accounts can do\. The first SCP prevents anyone in any of the member accounts from creating or modifying any AWS CloudTrail logs that you configure\. The management account isn't affected by any SCP, so after you apply the CloudTrail SCP, you must create any logs from the management account\.

### Enable the service control policy type in the organization<a name="tutorial-orgs-step3-enable-scp"></a>

Before you can attach a policy of any type to a root or to any OU within a root, you must enable the policy type for organization\. Policy types aren't enabled by default\. The steps in this section show you how to enable the service control policy \(SCP\) type for your organization\.

**To enable SCPs for your organization**

1. In the navigation pane, choose **Policies**, and then on the **Policies** page, choose **Service Control Policies**\.

1. On the **Service Control Policies** page, choose **Enable service control policies**\.

   A green banner appears to inform you that you can now create SCPs in your organization\.

### Create your SCPs<a name="tutorial-orgs-step3-create-pols"></a>

Now that service control policies are enabled in your organization, you can create the three policies that you need for this tutorial\.

**To create the first SCP that blocks CloudTrail configuration actions**

1. In the navigation pane, choose **Policies**, and then on the **Policies** page, choose **Service Control Policies**\.

1. On the **Service Control Policies** page , choose **Create policy**\.

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

### Attach the SCPs to your OUs<a name="tut-basic-attach-scp"></a>

Now that the SCPs exist and are enabled for your root, you can attach them to the root and OUs\.

**To attach the policies to the root and the OUs**

1. Navigate to the **AWS Accounts** page\.

1. On the **AWS Accounts** page, choose **Root** \(its name, not the radio button\) to navigate to its details page\.

1. On the **Root** details page, choose the **Policies** tab\., and then Under **Service Control Policies**, choose **Attach**\.

1. On the **Attach a service control policy** page, choose the radio button next to the SCP named `Block CloudTrail Configuration Actions`\. In this tutorial, you attach it to the root so that it affects all member accounts to prevent anyone from altering the way that you configured CloudTrail\. 

   The **Root** details page, **Policies** tab now shows that two SCPs are attached to the root: the one you just created and the default `FullAWSAccess` SCP\. 

1. Navigate back to the **AWS Accounts** page, and choose the **Production** OU \(it's name, not the radio button\) to navigate to its details page\.

1. On the **Production** OU's details page, choose the **Policies** tab\. 

1. Under **Service Control Policies**, choose **Attach**\.

1. On the **Attach a service control policy** page, choose the radio button next to `Allow List for All Approved Services`, and then choose **Attach**\. This enables users or roles in member accounts in the `Production` OU to access the approved services\.

1. Choose the **Policies** tab again to see that two SCPs are attached to the OU: the one that you just attached and the default `FullAWSAccess` SCP\. However, because the `FullAWSAccess` SCP is also an allow list that allows all services and actions, you must now detach this SCP to ensure that only your approved services are allowed\.

1. To remove the default policy from the `Production` OU, choose the radio button  to **FullAWSAccess**, choose **Detach**, and then on the confirmation dialog box, choose **Detach policy**\.

   After you remove this default policy, all member accounts under the root immediately lose access to all actions and services that are not on the allow list SCP that you attached in the preceding steps\. Any requests to use actions that aren't included in the **Allow List for All Approved Services** SCP are denied\. This is true even if an administrator in an account grants access to another service by attaching an IAM permissions policy to a user in one of the member accounts\.

1. Now you can attach the SCP named `Deny List for MainApp Prohibited services` to prevent anyone in the accounts in the MainApp OU from using any of the restricted services\.

   To do this, navigate to the AWS Accounts page, choose the triangle icon to expand the Production OU's branch, and then choose the **MainApp** OU \(it's name, not the radio button\) to navigate to its contents\.

1. On the **MainApp** details page, choose the **Policies** tab\.

1. Under **Service Control Policies**, choose Attach, and then in the list of available policies, choose the radio button next to **Deny List for MainApp Prohibited Services**, and then choose **Attach policy**\.

## Step 4: Testing your organization's policies<a name="tutorial-orgs-step4"></a>

You now can sign in as a user in any of the member accounts and try to perform various AWS actions:
+ If you sign in as a user in the management account, you can perform any operation that is allowed by your IAM permissions policies\. The SCPs don't affect any user or role in the management account, no matter which root or OU the account is located in\.
+ If you sign in as the root user or an IAM user in account 222222222222, you can perform any actions that are allowed by the allow list\. AWS Organizations denies any attempt to perform an action in any service that isn't in the allow list\. Also, AWS Organizations denies any attempt to perform one of the CloudTrail configuration actions\.
+ If you sign in as a user in account 333333333333, you can perform any actions that are allowed by the allow list and not blocked by the deny list\. AWS Organizations denies any attempt to perform an action that isn't in the allow list policy and any action that is in the deny list policy\. Also, AWS Organizations denies any attempt to perform one of the CloudTrail configuration actions\.