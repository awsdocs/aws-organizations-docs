# Tag policies<a name="orgs_manage_policies_tag-policies"></a>

For information and procedures common to all policy types, see the following topics:
+ [Enable and disable policy types](orgs_manage_policies_enable-disable.md)
+ [Get details about your policies](orgs_manage_policies_info-operations.md)
+ [Policy syntax and inheritance](orgs_manage_policies_inheritance_auth.md)

You can use tag policies to maintain consistent tags, including the preferred case treatment of tag keys and tag values\.

## What are tags?<a name="what-are-tags"></a>

*Tags* are custom attribute labels that you assign or that AWS assigns to AWS resources\. Each tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, or `Project`\)\. Tag keys are case sensitive\.
+ An optional field known as a *tag value* \(for example, `111122223333` or `Production`\)\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

The rest of this page describes tag policies\. For more information about tags, see the following sources:
+ For more general information on tagging, including naming and usage conventions, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.
+ For a list of services that support using tags, see the [https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\.
+ For information on tagging Organizations resources, see [Tagging AWS Organizations resources](orgs_tagging.md)\.
+ For information on tagging resources in other AWS services, see the documentation for each\.
+ For information about using tags to categorize resources, see [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\.

## What are tag policies?<a name="what-are-tag-policies"></a>

*Tag policies* are a type of policy that can help you standardize tags across resources in your organization's accounts\. In a tag policy, you specify tagging rules applicable to resources when they are tagged\.

For example, a tag policy can specify that when the `CostCenter` tag is attached to a resource, it must use the case treatment and tag values that the tag policy defines\. A tag policy can also specify that noncompliant tagging operations on specified resource types are *enforced*\. In other words, noncompliant tagging requests on specified resource types are prevented from completing\. Untagged resources or tags that aren't defined in the tag policy aren't evaluated for compliance with the tag policy\.

Using tag policies involves working with multiple AWS services:
+ Use **AWS Organizations** to manage *tag policies*\. When signed in to the organization's master account, you use Organizations to enable the tag policies feature\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\. Then you can create tag policies and attach them to the organization entities to put those tagging rules in effect\. 
+ Use **AWS Resource Groups** to manage *compliance* with tag policies\. When signed in to an account in your organization, you use Resource Groups to find noncompliant tags on resources in the account\. You can correct noncompliant tags in the AWS service where you created the resource\. 

  If you sign in to the master account in your organization, you can view compliance information for all your organization's accounts\.

Tag policies are available only in an organization that has [all features enabled](orgs_manage_org_support-all-features.md)\. For more information on what's required to use tag policies, see [Prerequisites and permissions for managing tag policies](orgs_manage_policies_tag-policies-prereqs.md)\.

**Important**  
To get started with tag policies, AWS strongly recommends that you follow the example workflow described in [Getting started with tag policies](orgs_manage_policies_tag-policies-getting-started.md) before moving on to more advanced tag policies\. It's best to understand the effects of attaching a simple tag policy to a single account before expanding tag policies to an entire OU or organization\. It's especially important to understand a tag policy's effects before you *enforce* compliance with any tag policy\. The tables on this page also provide links to instructions for more advanced policy\-related tasks\.