# Understanding enforcement<a name="orgs_manage_policies_tag-policies-enforcement"></a>

A tag policy can specify that noncompliant tagging operations on specified resource types are *enforced*\. In other words, noncompliant tagging requests on specified resource types are prevented from completing\.

**Important**  
Enforcement has no effect on resources that are created without tags\.

To enforce compliance with tag policies, do one of the following when you [create a tag policy](orgs_manage_policies_tag-policies-create.md):
+ From the **Visual editor** tab, select [**Prevent noncompliant operations for this tag**](orgs_manage_policies_tag-policies-create.md#prevent-noncompliant-operations)\.
+ From the **JSON** tab, use the `enforced_for` field\. For information on tag policy syntax, see [Tag policy syntax and examples](orgs_manage_policies_example-tag-policies.md)\.

Follow these best practices for enforcing compliance with tag policies:
+ **Use caution in enforcing compliance** – Make sure you understand the effects of using tag policies, and follow the recommended workflows described in [Getting started with tag policies](orgs_manage_policies_tag-policies-getting-started.md)\. Test how enforcement works on a test account before expanding it to more accounts\. Otherwise, you could prevent users in your organization's accounts from tagging the resources they need\.
+ **Be aware of what resource types you can enforce on** – You can only enforce compliance with tag policies on [supported resource types](orgs_manage_policies_supported-resources-enforcement.md)\. Resource types that support enforcing compliance are listed when you use the visual editor to build a tag policy\. 
+ **Understand interactions with some services ** – Some AWS services have container\-like groupings of resources that automatically create resources for you, and tags can propagate from a resource in one service to another\. For example, tags on Amazon EC2 Auto Scaling groups and Amazon EMR clusters can automatically propagate to the contained Amazon EC2 instances\. You may have tag policies for Amazon EC2 that are more strict than for Auto Scaling groups or EMR clusters\. If you enable enforcement, the tag policy prevents resources from being tagged and may block dynamic scaling and provisioning\.