# Tagging AWS Organizations resources<a name="orgs_tagging"></a>

A *tag* is a custom attribute label that you add to an AWS resource to make it easier to identify, organize, and search for resources\. Each tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, or `Project`\)\. Tag keys are case sensitive\.
+ A *tag value* \(for example, `111122223333` or `Production`\)\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

You can use tags to categorize resources by purpose, owner, environment, or other criteria\. For more information, see [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\.

**Tip**  
Use [tag policies](orgs_manage_policies_tag-policies.md) to help standardize your implementation of tags across the resources in your organization's accounts\.

Currently, AWS Organizations supports the following tagging operations when you are logged in to the management account:
+ You can add tags to the following organization resources:
  + AWS accounts
  + Organizational units
  + The organization's root
  + Policies

You can add tags at the following times:
+ [When you create the resource](#add-tag-new) — Specify the tags in either the Organizations console, or use the `Tags` parameter with one of the `Create` API operations\. This isn't applicable to the organization's root\. 
+ [After you create the resource](#add-tag-existing) — Use the Organizations console, or call the `[TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html)` operation\.

You can view the tags on any of the taggable resources in AWS Organizations by using the console or by calling the `[ListTagsForResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListTagsForResource.html)` operation\.

You can remove tags from a resource by specifying the keys to remove by using the console or by calling the `[UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)` operation\.

## Using tags<a name="use-tags"></a>

Tags help you to organize your resources by enabling you to group them by things by whatever categories are useful to you\. For example, you can assign a "Department" tag that tracks the owning department\. You can assign an "Environment" tag to track whether a given resource is part of your alpha, beta, gamma, or production environments\.  
+ You can [enforce tagging standards on your resources by using tag policies](orgs_manage_policies_tag-policies.md)\.
+ Tags can help you to [control who can access and manage the components that make up your organization](orgs_tagging_tbac.md)\. 

## Adding tags<a name="add-tag"></a>

When signed in with permissions to your organization's management account, you can add tags to the resources in your organization\. 

### Adding tags to a resource when you create it<a name="add-tag-new"></a>

**Minimum permissions**  
To add tags to a resource when you create it, you need the following permissions:  
Permission to create a resource of the specified type
`organizations:TagResource`
`organizations:ListTagsForResource` – required only when using the Organizations console

You can include tag keys and values that are attached to the following resources as you create them\.
+ AWS account
  + [Created account](orgs_manage_accounts_create.md)
  + [Invited account](orgs_manage_accounts_invites.md#orgs_manage_accounts_invite-account)
+ [Organizational unit \(OU\)](orgs_manage_ous.md#create_ou)
+ Policy
  + [AI services opt\-out policy](orgs_manage_policies_ai-opt-out_create.md#create-ai-opt-out-policy-procedure)
  + [Backup policy](orgs_manage_policies_backup_create.md#create-backup-policy-procedure)
  + [Service control policy](orgs_manage_policies_scps_create.md#create-an-scp)
  + [Tag policy](orgs_manage_policies_tag-policies-create.md#create-tag-policy-procedure)

The organization root is created when you initially create the organization, so you can only add tags to it as an existing resource\.

### Adding tags to an existing resource<a name="add-tag-existing"></a>

You can also add new tags or update the values of tags attached to existing resources\.

**Minimum permissions**  
To add tags to resources in your organization, you need the following permissions:  
`organizations:TagResource`
`organizations:ListTagsForResource` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To add tags to an existing resource**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to and choose the account, root, OU, or policy, and click on its name to open its detail page\.

1. Choose the **Tags** tab, and then choose **Manage tags**\.

1. Choose **Add tag**, and then enter a **Key** and, optionally, a **Value** for the tag\.

   Tag keys and values are case sensitive\. Use the capitalization that you want to standardize on\. You must also comply with the requirements of any tag policies that apply\.

1. Repeat step 4 as many times as you need\.

1. Choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To add tags to an existing resource**  
You can use one of the following commands to add tags to the taggable resources in your organization:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html)

------

## Viewing tags on resources in your organization<a name="view-tag"></a>

When signed in with permissions to your organization's management account, you can view tags on taggable resources in your organization\.

**Minimum permissions**  
To view a resource's tags, you need the following permissions:  
`organizations:ListTagsForResource`

------
#### [ AWS Management Console ]

**To view tags on a resource in your organization**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to and choose the account, root, OU, or policy, and click on its name to open its detail page\.

1. Choose the **Tags** tab\.

   All tags that are attached to the selected resource are displayed\.

------
#### [ AWS CLI & AWS SDKs ]

**To view tags on a resource in your organization**  
You can use one of the following commands to view tags on a resource in your organization:
+ AWS CLI: [aws organizations list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-tags-for-resource.html)
+ AWS SDKs: [ListTagsForResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListTagsForResource.html)

------

## Editing tag values<a name="edit-tag"></a>

When signed in with permissions to your organization's management account, you can edit the *tag values* for tags that are attached to your taggable resources\.

**Note**  
You can't change a tag **key**\. You can delete the tag with the old key and then add a tag with the new key\. For more information, see [Deleting tags](#delete-tag) and [Adding tags](#add-tag)\.

**Minimum permissions**  
To edit tag **values** on tags that are attached to your taggable resources, you need the following permissions:  
`organizations:ListTagsForResource`
`organizations:TagResource`
`organizations:ListTagsForResource` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To edit the tags attached to resource in your organization**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to and choose the account, root, OU, or policy, and click on its name to open its detail page\.

1. Choose the **Tags** tab, and then choose **Manage Tags**\.

1. You can add new tags, modify the values of existing tags, or remove tags\.

1. Choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To edit the tag value for a tag on a resource in your organization**  
You can the following commands to modify the value for a tag attached to the taggable resources in your organization\. When you specify a tag key that already exists, the value for that key is overwritten:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html)

------

## Deleting tags<a name="delete-tag"></a>

When signed in with permissions to your organization's management account, you can delete tags that are attached to taggable resources in your organization\. 

**Note**  
To delete tags, you need the following permissions:
`organizations:ListTagsForResource` – required only when using the Organizations console
`organizations:UntagResource` 

------
#### [ AWS Management Console ]

**To delete tags from a resource in your organization**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to and choose the account, root, OU, or policy, and click on its name to open its detail page\.

1. Choose the **Tags** tab, and then choose **Manage tags**\.

1. Choose **Remove** next to the tag to delete it\.

1. Choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To delete tags from a resource in your organization**  
You can use one of the following commands to delete tags:
+ AWS CLI: [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS SDKs: [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------