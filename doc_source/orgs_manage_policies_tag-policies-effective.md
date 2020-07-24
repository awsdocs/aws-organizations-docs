# Viewing effective tag policies<a name="orgs_manage_policies_tag-policies-effective"></a>

Before you start checking compliance status for tagged resources in an account, it's helpful to first determine the effective tag policy for an account\.

## What is the effective tag policy?<a name="effective-tag-policy-defined"></a>

The *effective tag policy* specifies the tagging rules that apply to an account\. It is the aggregation of any tag policies the account inherits, plus any tag policy directly attached to the account\. When you attach a tag policy to the organization root, it applies to all accounts in your organization\. When you attach a tag policy to an OU, it applies to all accounts and OUs that belong to the OU\. 

For example, the tag policy attached to the organization root may define a `CostCenter` tag with five compliant values\. A separate tag policy attached to the account may restrict the `CostCenter` key to only two of the four compliant values\. The combination of these tag policies comprises the effective tag policy\. The result is that only two of the four compliant tag values defined in the organization root tag policy are compliant for the account\.

For more information and more advanced examples of how effective tag policies are generated, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

## How to view the effective tag policy<a name="how-to-view-effective-tag-policy"></a>

You can view the effective tag policy for an account from the AWS Management Console, AWS API, or AWS Command Line Interface\.

To view the effective tag policy for an account, you must have permission to run the following actions:
+ `organizations:DescribeEffectivePolicy`
+ `organizations:DescribeOrganization`

**To view the effective policy for an account \(console\)**

1. Sign in to the organization's master account\.
**Note**  
When you are signed in to a member account, the procedure for viewing the effective policy is different\. When signed in to an account, you can view the effective tag policy in the context of evaluating compliance for the account\. For more information, see [ Evaluating Compliance for an Account](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-arg-finding-noncompliant-tags.html) in the *AWS Resource Groups User Guide\.*

1. On the **Accounts** tab, choose the account\.

1. In the details pane on the right, expand the **Tag policies** section\.

1. Choose **View effective policy**\.

**To view the effective policy for an account \(AWS CLI, AWS API\)**  
You can use one of the following to view the effective tag policy:
+ AWS CLI: [aws organizations describe\-effective\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-effective-policy.html)

  For the complete procedure for using tag policies in the AWS CLI, see [Using tag policies in the AWS CLI](tag-policy-cli.md)\.
+ AWS API: [DescribeEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeEffectivePolicy.html)