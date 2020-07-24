# Managing organizational units \(OUs\)<a name="orgs_manage_ous"></a>

You can use organizational units \(OUs\) to group accounts together to administer as a single unit\. This greatly simplifies the management of your accounts\. For example, you can attach a policy\-based control to an OU, and all accounts within the OU automatically inherit the policy\. You can create multiple OUs within a single organization, and you can create OUs within other OUs\. Each OU can contain multiple accounts, and you can move accounts from one OU to another\. However, OU names must be unique within a parent OU or root\.

**Note**  
Currently, you can have only a single root, which AWS Organizations creates for you when you first set up your organization\. The name of the default root is "root\."

To structure the accounts in your organization, you can perform the following tasks:
+ [Viewing details of an OU](orgs_manage_org_details.md#orgs_view_ou) 
+ [Creating an OU](#create_ou)
+ [Renaming an OU](#rename_ou)
+ [Moving an account to an OU or between the root and OUs](#move_account_to_ou)

## Navigating the root and OU hierarchy<a name="navigate_tree"></a>

To navigate to different OUs or to the root when moving accounts or attaching policies, you can use the tree view\.

**To enable and use the tree view of the organization**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. Choose the **Organize accounts** tab\.

1. If the tree view pane isn't visible on the left side of the page, choose the **TREE VIEW** switch icon ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/switch-icon.png)\.

1. The tree initially appears showing the root, with only the first level of child OUs displayed\. To expand the tree to show deeper levels, choose the \+ icon next to any parent entity\. To reduce clutter and collapse a branch of the tree, choose the — icon next to an expanded parent entity\.

1. Choose the OU or root that you want to navigate to\. The node in the tree view that is displayed in bold text is the one that you are currently viewing in the center pane\.

**Notes**  
**Rename, Delete, and Move operations in the center pane**: When you view the contents of a root or OU in the console, you can interact with the child entities of that root or OU\. For example, if you select the check box for a child OU or account, you can choose the **Rename**, **Delete**, or **Move** links above that section to perform those operations on the selected entity\. The operations apply *only* to the child entities that you select\. They don't apply to the containing root or OU\. To perform the same operations for the containing OU, you must navigate to the OU's parent OU or root, and then select the check box for the child OU that you want to manage\.
**Details pane**: The details pane on the right side of the console shows information about the root or OU that you are viewing\. If you select a check box for a child entity, the details pane switches to show information about the selected entity\. To see the details of the containing root or OU again, you must clear the check box\. Alternatively, you can navigate to the parent root or OU, and then select the check box for the OU whose information you want to see\.

**To navigate without using the tree view**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\.

1. Choose the **Organize accounts** tab\.

1. Navigate down a branch by choosing the name of the OU \(not the check box\) that you want to view in the center pane\.

1. Navigate up the branch by choosing the back button \(<\) on the title bar of the center pane\.

## Creating an OU<a name="create_ou"></a>

When signed in to your organization's master account, you can create an OU in your organization's root\. OUs can be nested up to five levels deep\. To create an OU, complete the following steps\.

**Important**  
If this organization is managed with AWS Control Tower, then create your OUs with the AWS Control Tower console or APIs\. If you create the OU in Organizations then that OU isn't registered with AWS Control Tower\. For more information, see [Referring to Resources Outside of AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/external-resources.html#ungoverned-resources) in the *AWS Control Tower User Guide*\.

**Minimum permissions**  
To create an OU within a root in your organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:CreateOrganizationalUnit`

**To create an OU \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

   The console displays the contents of the root\. The first time you visit a root, the console displays all of your AWS accounts in that top\-level view\. If you previously created OUs and moved accounts into them, the console shows only the top\-level OUs and any accounts that you have not yet moved into an OU\.

1. \(Optional\) If you want to create an OU inside an existing OU, [navigate to the child OU](#navigate_tree) by choosing the name \(not the check box\) of the child OU, or by choosing the OU in the tree view\.

1. When you're in the correct location in the hierarchy, choose **Create organizational unit \(OU\)**\.

1. In the **Create organizational unit** dialog box, type the name of the OU that you want to create, and then choose **Create organizational unit**\.

   Your new OU appears inside the parent\. You now can [move accounts to this OU](#move_account_to_ou) or attach policies to it\.

**To create an OU \(AWS CLI, AWS API\)**  
You can use one of the following commands to create an OU:
+ AWS CLI: [aws organizations create\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-organizational-unit.html)
+ AWS API: [CreateOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateOrganizationalUnit.html)

## Renaming an OU<a name="rename_ou"></a>

When signed in to your organization's master account, you can rename an OU\. To do this, complete the following steps\.

**Minimum permissions**  
To rename an OU within a root in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:UpdateOrganizationalUnit`

**To rename an OU \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to the parent of the OU](#navigate_tree) that you want to rename\. Select the check box for the child OU that you want to rename\.

1. Choose **Rename** above the list of OUs\.

1. In the **Rename organizational unit** dialog box, type a new name, and then choose **Rename organizational unit**\.

**To rename an OU \(AWS CLI, AWS API\)**  
You can use one of the following commands to rename an OU:
+ AWS CLI: [aws organizations update\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-organizational-unit.html)
+ AWS API: [UpdateOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdateOrganizationalUnit.html)

## Moving an account to an OU or between the root and OUs<a name="move_account_to_ou"></a>

When signed in to your organization's master account, you can move accounts in your organization from the root to an OU, from one OU to another, or back to the root from an OU\. Placing an account inside an OU makes it subject to any policies that are attached to the parent OU and any other OUs in the parent chain up to the root\. If an account isn't in an OU, it's subject to only the policies that are attached to the root and any that are attached directly to the account\. To move an account, complete the following steps\.

**Minimum permissions**  
To move an account to a new location in the OU hierarchy, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:MoveAccount`

**To move an account to an OU \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Choose the **Organize accounts** tab and then [navigate to the OU](#navigate_tree) that contains the account that you want to move\. When you find the account, select its check box\. Select multiple check boxes if you want to move multiple accounts\.

1. Choose **Move ** above the list of accounts\.

1. In the **Move accounts** dialog box, choose the OU or the root that you want to move the accounts to and then choose **Select**\.

**To move an account to an OU \(AWS CLI, AWS API\)**  
You can use one of the following commands to move an account:
+ AWS CLI: [aws organizations move\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/move-account.html)
+ AWS API: [MoveAccount](https://docs.aws.amazon.com/organizations/latest/APIReference/API_MoveAccount.html)

## Deleting an OU that you no longer need<a name="delete-ou"></a>

When signed in to your organization's master account, you can delete OUs that you no longer need\. You first must move all accounts out of the OU and any child OUs, and then delete the child OUs\.

**Minimum permissions**  
To delete an OU, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only\)
`organizations:DeleteOrganizationalUnit`

**To delete an OU \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate to the parent container](#navigate_tree) of the OU that you want to delete\. Select the OU's check box\. You can select check boxes for multiple OUs if you want to delete more than one\.

1. Choose **Delete** above the list of OUs\.

   AWS Organizations deletes the OU and removes it from the list\.

**To delete an OU \(AWS CLI, AWS API\)**  
You can use one of the following commands to delete an OU: 
+ AWS CLI: [aws organizations delete\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-organizational-unit.html)
+ AWS API: [DeleteOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeleteOrganizationalUnit.html)