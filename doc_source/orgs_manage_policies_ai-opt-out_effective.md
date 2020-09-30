# Viewing effective AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_effective"></a>

Determine the effective Artificial Intelligence \(AI\) services opt\-out policy for an account in your organization\.

## What is the effective AI services opt\-out policy?<a name="effective-ai-opt-out-policy-defined"></a>

The *effective AI services opt\-out policy* specifies the final rules that apply to an account\. It is the aggregation of any AI services opt\-out policies that the account inherits, plus any AI services opt\-out policies that are directly attached to the account\. When you attach an AI services opt\-out policy to the organization root, it applies to all accounts in your organization\. When you attach an AI services opt\-out policy to an OU, it applies to all accounts and OUs that belong to the OU\. When you attach a policy directly to an account, it applies only to that one account\.

For example, the AI services opt\-out policy attached to the organization root might specify that all accounts in the organization opt out of content use by all AWS machine learning services\. A separate AI services opt\-out policy attached directly to one member account specifies that it opts in to content use for only Amazon Rekognition\. The combination of these AI services opt\-out policies comprises the effective AI services opt\-out policy\. The result is that all accounts in the organization are opted out of all AWS services, with the exception of one account that opts in to Amazon Rekognition\.

For information about how policies are combined into the final effective policy, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

## How to view the effective AI services opt\-out policy<a name="how-to-view-effective-tag-policy"></a>

You can view the effective AI services opt\-out policy for an account from the AWS Management Console, AWS API, or AWS Command Line Interface\.

**Minimum permissions**  
To view the effective AI services opt\-out policy for an account, you must have permission to run the following actions:  
`organizations:DescribeEffectivePolicy`
`organizations:DescribeOrganization`

------
#### [ AWS Management Console ]

**To view the effective AI services opt\-out policy for an account**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\. 

1. On the **Accounts** tab, choose the account\.

1. In the details pane on the right, expand the **AI services opt\-out policies** section\.

1. Choose **View effective policy**\.

------
#### [ AWS CLI, AWS API ]

**To view the effective policy for an account**  
You can use one of the following to view the effective AI services opt\-out policy:
+ AWS CLI: [aws organizations describe\-effective\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-effective-policy.html)

  For the complete procedure for using AI services opt\-out policies in the AWS CLI, see [Using tag policies in the AWS CLI](tag-policy-cli.md)\.
+ AWS API: [DescribeEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeEffectivePolicy.html) 

------