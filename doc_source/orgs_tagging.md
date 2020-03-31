# Tagging AWS Organizations resources<a name="orgs_tagging"></a>

A *tag* is a custom attribute label that you add to an AWS resource to make it easier to identify, organize, and search for resources\. Each tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, or `Project`\)\. Tag keys are case sensitive\.
+ A *tag value* \(for example, `111122223333` or `Production`\)\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

You can use tags to categorize resources by purpose, owner, environment, or other criteria\. For more information, see [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\.

**Tip**  
 You can use [tag policies](orgs_manage_policies_tag-policies.md) to help standardize tags across resources in your organization's accounts\.

## Supported resources in AWS Organizations<a name="supported-resources"></a>

Currently, AWS Organizations supports the following tagging operations when you are logged in to the master account:
+ You can tag and untag accounts in AWS Organizations\.
+ You can view tags on an account in AWS Organizations\.

AWS Organizations doesn't currently support tagging resources within an account, cost allocation tags, or the tag\-based access control feature of AWS Identity and Access Management \(IAM\)\.

## Adding tags<a name="add-tag"></a>

When signed in with permissions to your organization's master account, you can add tags to accounts in your organization\. 

To add tags to accounts in your organization, you need permission to run the following actions:
+ `organizations:ListTagsForResource` \(console only\)
+ `organizations:TagResource`

**To add a tag to an account in your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose an account\.

1. In the **TAGS** section of the details pane on the right, choose **EDIT TAGS**\.

1. Enter a key and, optionally, a value for the tag\.

   Tag keys and values are case sensitive\. Use the capitalization that you want to standardize on\. 

1. Choose **Save changes**\.

Any tags that you added to the account appear in the **TAGS** section of the details pane on the right\.

**To add a tag to an account in your organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to add tags to accounts:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html)
+ AWS API: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html)

## Viewing tags on an account<a name="list-tagged-resources"></a>

When signed in with permissions to your organization's master account, you can view tags on an account in your organization\.

To view tags on an account in your organization, you need permission to run the following action:
+ `organizations:ListTagsForResource`

**To view tags on an account in your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose an account\.

1. In the details pane on the right, find the **TAGS** section\.

All tags that are attached to the selected account are displayed\.

**To view tags on an account in your organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to view tags on an account:
+ AWS CLI: [aws organizations list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-tags-for-resource.html)
+ AWS API: [ListTagsForResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListTagsForResource.html)

## Editing tag values<a name="edit-tag"></a>

When signed in with permissions to your organization's master account, you can edit *tag values* on tags that are attached to accounts\.

To edit *tag keys*, you need to delete the tag key and then add a new tag key\. For more information, see [Deleting tags](#delete-tag) and [Adding tags](#add-tag)\.

To edit tag values on tags that are attached to accounts, you need permission to run the following actions:
+ `organizations:ListTagsForResource`
+ `organizations:TagResource`

**To edit a tag value for a tag on an account in your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose an account\.

1. In the **TAG** section of the details pane on the right, choose **EDIT TAGS**\.

1. Modify the value of the tag that you want to change\.

1. Choose **Save changes**\.

The **TAGS** section in the details pane on the right updates with any changes that you made to tag values for tags on the account\. 

**To edit a tag value on an account in an organization \(AWS CLI, AWS API\)**

1. Delete the existing tag value by using one of the following commands:
   + AWS CLI: [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
   + AWS API: [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

1. Add a new tag value by using one of the following commands:
   + AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html)
   + AWS API: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html)

## Deleting tags<a name="delete-tag"></a>

When signed in with permissions to your organization's master account, you can delete tags that are attached to accounts in your organization\. 

To delete tags, you need permission to run the following actions:
+ `organizations:ListTagsForResource`
+ `organizations:UntagResource` 

**To delete a tag from an account in your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Accounts** tab, choose an account\.

1. In the **TAGS** section of the details pane on the right, choose **EDIT TAGS**\.

1. Choose **Remove** next to the tag to delete it\.

1. Choose **Save changes**\.

The **TAGS** section in the details pane no longer displays the tags that you deleted\. 

**To delete a tag from an account in your organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to delete tags:
+ AWS CLI: [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS API: [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)