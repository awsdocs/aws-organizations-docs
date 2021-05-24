# Tagging AWS Organizations resources<a name="orgs_tagging"></a>

A *tag* is a custom attribute label that you add to an AWS resource to make it easier to identify, organize, and search for resources\. Each tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, or `Project`\)\. Tag keys can be up to 128 characters in length and are case sensitive\.
+ A *tag value* \(for example, `111122223333` or `Production`\)\. Tag values can be up to 256 characters in length, and like tag keys, are case sensitive\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. Omitting the tag value is the same as using an empty string\. 

For more information about what characters are allowed in a tag key or value, see the [Tags parameter of the Tag API](https://docs.aws.amazon.com/ARG/latest/APIReference/API_Tag.html#ARG-Tag-request-Tags) in the *Resource Groups Tagging API Reference*\.

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
+ Tags can help you to [control who can access and manage the components that make up your organization](orgs_tagging_abac.md)\. 

## Adding, updating, and removing tags<a name="add-tag"></a>

When you sign in to your organization's management account, you can add tags to the resources in your organization\. 

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

### Adding or updating tags for an existing resource<a name="add-tag-existing"></a>

You can also add new tags or update the values of tags attached to existing resources\.

**Minimum permissions**  
To add or update tags to resources in your organization, you need the following permissions:  
`organizations:TagResource`
`organizations:ListTagsForResource` – required only when using the Organizations console
To remove tags from resources in your organization, you need the following permissions:  
`organizations:UntagResource`

------
#### [ AWS Management Console ]

**To add, update, or remove tags for an existing resource**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to and choose the account, Root, OU, or policy, and click on its name to open its detail page\.

1. On the **Tags** tab, choose **Manage tags**\.

1. You can add new tags, modify the values of existing tags, or remove tags\.

   To add a tag, choose **Add tag**, and then enter a **Key** and, optionally, a **Value** for the tag\.

   To remove a tag, choose **Remove**\.

   Tag keys and values are case sensitive\. Use the capitalization that you want to standardize on\. You must also comply with the requirements of any tag policies that apply\.

1. Repeat the previous step as many times as you need\.

1. Choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To add or update tags to an existing resource**  
You can use one of the following commands to add tags to the taggable resources in your organization:
+ AWS CLI: [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html)

**To delete tags from a resource in your organization**  
You can use one of the following commands to delete tags:
+ AWS CLI: [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS SDKs: [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------