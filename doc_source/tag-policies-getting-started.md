# Getting Started with Tag Policies<a name="tag-policies-getting-started"></a>

Using tag policies involves working with multiple AWS services\. To get started, review the following pages\. Then follow the workflows on this page to get familiar with tag policies and their effects\.
+ [Prerequisites and Permissions for Managing Tag Policies](orgs_manage_policies_tag-policies-prereqs.md)
+ [Best Practices for Using Tag Policies](orgs_manage_policies_tag-policies-best-practices.md)

## Using Tag Policies for the First Time<a name="getting-started-first-time"></a>

Follow these steps to get started using tag policies for the first time\.


| Task | How to Perform | 
| --- | --- | 
|  Step 1: [Enable tag policies for your organization\.](enable-tag-policies.md)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
|  Step 2: [Create a tag policy](orgs_manage_policies_tag-policies-create.md)\.  Keep your first tag policy simple\. Enter one tag key in the case treatment you want to use and leave all other options at their defaults\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
|  Step 3: [Attach a tag policy to a single member account that you can use for testing\.](attach-tag-policy.md) You'll need to sign in to this account in the next step\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| Step 4: Create some resources with compliant tags and some with noncompliant tags\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
|  Step 5: [ View the effective tag policy and evaluate the compliance status of the account\.](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html) If you created resources with compliant and noncompliant tags, you should see the noncompliant tags in the results\.  | 
|  Step 6: Repeat the process of finding and correcting compliance issues until the resources in the test account are compliant with your tag policy\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| At any time, you can [ evaluate organization\-wide compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html)\. | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html) | 

¹ You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

If you're using the AWS Command Line Interface, the complete process and examples are described in [Using Tag Policies in the AWS CLI](tag-policy-cli.md)\. 

## Expanding Use of Tag Policies<a name="getting-started-more-advanced"></a>

You can perform the following tasks in any order to expand your use of tag policies\.


| Advanced Task |  How to Perform | 
| --- | --- | 
|  [Create more advanced tag policies](orgs_manage_policies_tag-policies-create.md)\. Follow the same process as for first\-time users, but try other tasks\. For example, define additional keys or values or specify different case treatment for a tag key\.  You can use the information in [How Policy Inheritance Works](orgs_manage_policies-inheritance.md) and [Tag Policy Syntax](orgs_manage_policies_example-tag-policies.md#tag-policy-syntax-reference) to create more detailed tag policies\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| [Attach tag policies to additional accounts or OUs\. ](attach-tag-policy.md) Check the [effective tag policy for an account](orgs_manage_policies_tag-policies-effective.md) after you attach more policies to it or to any OU in which the account is a member\. | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html) | 
| Create an SCP to require tags when anyone creates new resources\. For an example, see [Example 13: Require a Tag Upon Resource Creation](orgs_manage_policies_example-scps.md#example-require-tag-on-create)\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| [ Continue to evaluate the compliance status of the account against the effective tag policy as it changes\. Correct noncompliant tags\. ](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html) | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| [ Evaluate organization\-wide compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html)\. | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html) | 

¹ You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

## Enforcing Tag Policies for the First Time<a name="getting-started-enforcement"></a>

To enforce tag policies for the first time, follow a workflow similar to using tag policies for the first time and use a test account\.

**Warning**  
Use caution in enforcing compliance\. Make sure that you understand the effects of using tag policies and follow the recommended workflow\. Test how enforcement works on a test account before expanding it to more accounts\. Otherwise, you could prevent users in your organization's accounts from tagging the resources they need\. For more information, see [Understanding Enforcement](orgs_manage_policies_tag-policies-enforcement.md)\. 


| Enforcement Tasks |  How to Perform | 
| --- | --- | 
|  Step 1: [Create a tag policy](orgs_manage_policies_tag-policies-create.md)\.  Keep your first enforced tag policy simple\. Enter one tag key in the case treatment you want to use, and choose the **Prevent noncompliant operations for this tag** option\. Then specify one resource type to enforce it on\. Continuing with our earlier example, you can choose to enforce it on Secrets Manager secrets\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
|  Step 2: [Attach a tag policy to a single, test account\.](attach-tag-policy.md)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| Step 3: Try creating some resources with compliant tags, and some with noncompliant tags\. You shouldn't be allowed to create a tag a resource of the type specified in the tag policy with a noncompliant tag\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
|  Step 4: [ Evaluate the compliance status of the account against the effective tag policy and correct noncompliant tags\. ](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
|  Step 5: Repeat the process of finding and correcting compliance issues until the resources in the test account are compliant with your tag policy\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html)  | 
| At any time, you can [ evaluate organization\-wide compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html)\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/organizations/latest/userguide/tag-policies-getting-started.html) | 

¹ You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.