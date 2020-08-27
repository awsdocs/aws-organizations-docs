# Getting started with tag policies<a name="orgs_manage_policies_tag-policies-getting-started"></a>

Using tag policies involves working with multiple AWS services\. To get started, review the following pages\. Then follow the workflows on this page to get familiar with tag policies and their effects\.
+ [Prerequisites and permissions for managing tag policies](orgs_manage_policies_tag-policies-prereqs.md)
+ [Best practices for using tag policies](orgs_manage_policies_tag-policies-best-practices.md)

## Using tag policies for the first time<a name="getting-started-first-time"></a>

Follow these steps to get started using tag policies for the first time\.


| Task | Account to sign in to | AWS service console to use | 
| --- | --- | --- | 
|  Step 1: [Enable tag policies for your organization\.](orgs_manage_policies_enable-disable.md)  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
| Step 2: [Create a tag policy](orgs_manage_policies_tag-policies-create.md)\. Keep your first tag policy simple\. Enter one tag key in the case treatment you want to use and leave all other options at their defaults\.  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  Step 3: [Attach a tag policy to a single member account that you can use for testing\.](attach-tag-policy.md) You'll need to sign in to this account in the next step\.  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  Step 4: Create some resources with compliant tags and some with noncompliant tags\.  |  The member account that you're using for testing purposes\.  |  Any AWS service that you are comfortable with\. For example, you can use [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/) and follow the procedure in [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) to create secrets with compliant and non\-compliant secrets\.  | 
|  Step 5: [ View the effective tag policy and evaluate the compliance status of the account\.](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html)  |  The member account that you're using for testing purposes\.  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/) and the AWS service where the resource was created\. If you created resources with compliant and non\-compliant tags, you should see the non\-compliant tags in the results\.  | 
|  Step 6: Repeat the process of finding and correcting compliance issues until the resources in the test account are compliant with your tag policy\.  |  The member account that you're using for testing purposes\.  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/) and the AWS service where the resource was created\.  | 
|  At any time, you can [ evaluate organization\-wide compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html)\.  |  The organization's master account\.¹  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/)  | 

¹ You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

If you're using the AWS Command Line Interface, the complete process and examples are described in [Using tag policies in the AWS CLI](tag-policy-cli.md)\. 

## Expanding use of tag policies<a name="getting-started-more-advanced"></a>

You can perform the following tasks in any order to expand your use of tag policies\.


| Advanced task | Account to sign in to | AWS service console to use | 
| --- | --- | --- | 
|  [Create more advanced tag policies](orgs_manage_policies_tag-policies-create.md)\. Follow the same process as for first\-time users, but try other tasks\. For example, define additional keys or values or specify different case treatment for a tag key\.  You can use the information in [Understanding policy inheritance](orgs_manage_policies_inheritance.md) and [Tag policy syntax](orgs_manage_policies_example-tag-policies.md#tag-policy-syntax-reference) to create more detailed tag policies\.  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  [Attach tag policies to additional accounts or OUs\. ](attach-tag-policy.md) Check the [effective tag policy for an account](orgs_manage_policies_tag-policies-effective.md) after you attach more policies to it or to any OU in which the account is a member\.  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  Create an SCP to require tags when anyone creates new resources\. For an example, see [Example: Require a tag on specified created resources](orgs_manage_policies_scps_examples.md#example-require-tag-on-create)\.  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  [ Continue to evaluate the compliance status of the account against the effective tag policy as it changes\. Correct noncompliant tags\. ](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html)  |  A member account with an effective tag policy\.  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/) and the AWS service where the resource was created\.  | 
|  [ Evaluate organization\-wide compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html)\.  |  The organization's master account\.¹  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/)  | 

¹ You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

## Enforcing tag policies for the first time<a name="getting-started-enforcement"></a>

To enforce tag policies for the first time, follow a workflow similar to using tag policies for the first time and use a test account\.

**Warning**  
Use caution in enforcing compliance\. Make sure that you understand the effects of using tag policies and follow the recommended workflow\. Test how enforcement works on a test account before expanding it to more accounts\. Otherwise, you could prevent users in your organization's accounts from tagging the resources they need\. For more information, see [Understanding enforcement](orgs_manage_policies_tag-policies-enforcement.md)\. 


| Enforcement tasks | Account to sign in to | AWS service console to use | 
| --- | --- | --- | 
|  Step 1: [Create a tag policy](orgs_manage_policies_tag-policies-create.md)\.  Keep your first enforced tag policy simple\. Enter one tag key in the case treatment you want to use, and choose the **Prevent noncompliant operations for this tag** option\. Then specify one resource type to enforce it on\. Continuing with our earlier example, you can choose to enforce it on Secrets Manager secrets\.  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  Step 2: [Attach a tag policy to a single, test account\.](attach-tag-policy.md)  |  The organization's master account\.¹  |  [AWS Organizations](https://console.aws.amazon.com/organizations/)  | 
|  Step 3: Try creating some resources with compliant tags, and some with noncompliant tags\. You shouldn't be allowed to create a tag a resource of the type specified in the tag policy with a noncompliant tag\.   |  The member account that you're using for testing purposes\.  | Any AWS service that you are comfortable with\. For example, you can use [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/) and follow the procedure in [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) to create secrets with compliant and non\-compliant secrets\. | 
|  Step 4: [ Evaluate the compliance status of the account against the effective tag policy and correct noncompliant tags\. ](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html)  |  The member account that you're using for testing purposes\.  |  Resource Groups and the AWS service where the resource was created\.  | 
|  Step 5: Repeat the process of finding and correcting compliance issues until the resources in the test account are compliant with your tag policy\.  |  The member account that you're using for testing purposes\.  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/) and the AWS service where the resource was created\.  | 
|  At any time, you can [ evaluate organization\-wide compliance](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-evaluating-org-wide-compliance.html)\.  |  The organization's master account\.¹  |  [Resource Groups](https://console.aws.amazon.com/resource-groups/)  | 

¹ You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.