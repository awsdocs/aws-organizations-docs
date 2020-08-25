# AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out"></a>

For information and procedures common to all policy types, see the following topics:
+ [Enable and disable policy types](orgs_manage_policies_enable-disable.md)
+ [Get details about your policies](orgs_manage_policies_info-operations.md)
+ [Policy syntax and inheritance](orgs_manage_policies_inheritance_auth.md)

Certain AWS artificial intelligence \(AI\) services, including Amazon CodeGuru Profiler, Amazon Comprehend, Amazon Lex, Amazon Polly, Amazon Rekognition, Amazon Textract, Amazon Transcribe, and Amazon Translate, may store and use customer content processed by those services for the development and continuous improvement of Amazon AI services and technologies\. As an AWS customer, you can choose to opt out of having your content stored or used for service improvements\. Instead of configuring this setting individually for each AWS account that your organization uses, you can configure an organization policy that enforces your setting choice on all accounts that are members of the organization\. You can choose to opt out of content storage and use for an individual AI service, or for all of the covered services at once\. You can query the effective policy applicable to each account to see the effects of your setting choices\.

**Important**  
When you specify an opt in or opt out preference for a service, that setting is global and applied to all AWS Regions\. Setting the value from within one AWS Region replicates to all other Regions\.
When you opt out of content use by an AWS AI service, that service deletes all of the associated historical content that was stored prior to setting the option\.

## Getting started with AI services opt\-out policies<a name="orgs_manage_policies-ai-opt-out_getting-started"></a>

Follow these steps to get started using Artificial Intelligence \(AI\) services opt\-out policies\.

1. [Enable AI services opt\-out policies for your organization](orgs_manage_policies_enable-disable.md)\.

1. [Create an AI services opt\-out policy](orgs_manage_policies_ai-opt-out_create.md)\.

1. [Attach the AI services opt\-out policy to your organization's root, OU, or account](orgs_manage_policies_ai-opt-out_attach.md)\.

1. [View the combined effective AI services opt\-out policy that applies to an account](orgs_manage_policies_ai-opt-out_effective.md)\.

For all of these steps, you sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

**Other information**
+ [Learn policy syntax for AI services opt\-out policies and see policy examples](orgs_manage_policies_ai-opt-out_syntax.md)
+ [Use the AWS Command Line Interface to manage your AI services opt\-out policies](orgs_manage_policies_ai-opt-out_cli.md)