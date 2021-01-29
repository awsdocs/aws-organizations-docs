# Viewing effective AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_effective"></a>

**Note**  
AWS Organizations is introducing a new version of the Organizations management console\. You can switch between the old console and the new console by choosing the link in the notice boxes at the top of the console\. We encourage you to try the new version and let us know what you think\. We want your feedback and read each submission\.

Determine the effective Artificial Intelligence \(AI\) services opt\-out policy for an account in your organization\.

## What is the effective AI services opt\-out policy?<a name="effective-ai-opt-out-policy-defined"></a>

The *effective AI services opt\-out policy* specifies the final rules that apply to an AWS account\. It is the aggregation of any AI services opt\-out policies that the account inherits, plus any AI services opt\-out policies that are directly attached to the account\. When you attach an AI services opt\-out policy to the organization's root, it applies to all accounts in your organization\. When you attach an AI services opt\-out policy to an OU, it applies to all accounts and OUs that belong to the OU\. When you attach a policy directly to an account, it applies only to that one AWS account\.

For example, the AI services opt\-out policy attached to the organization root might specify that all accounts in the organization opt out of content use by all AWS machine learning services\. A separate AI services opt\-out policy attached directly to one member account specifies that it opts in to content use for only Amazon Rekognition\. The combination of these AI services opt\-out policies comprises the effective AI services opt\-out policy\. The result is that all accounts in the organization are opted out of all AWS services, with the exception of one account that opts in to Amazon Rekognition\.

For information about how policies are combined into the final effective policy, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

## How to view the effective AI services opt\-out policy<a name="how-to-view-effective-tag-policy"></a>

You can view the effective AI services opt\-out policy for an account from the AWS Management Console, AWS API, or AWS Command Line Interface\.

**Minimum permissions**  
To view the effective AI services opt\-out policy for an account, you must have permission to run the following actions:  
`organizations:DescribeEffectivePolicy`
`organizations:DescribeOrganization` – required only when using the Organizations console

------
#### [ Old console ]

**To view the effective AI services opt\-out policy for an account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Accounts](https://console.aws.amazon.com/organizations/home#/accounts)** tab, choose the account for which you want to view the effective AI services opt\-out policy\. 

   Alternatively, you can use the **[Organize accounts](https://console.aws.amazon.com/organizations/home#/browse)** tab, You might have to expand OUs \(choose the \+ next to an OU name\) in the navigation pane to find the account that you want\. Choose the account for which you want to view the effective AI services opt\-out policy\. 

1. In the details pane, choose **View effective AI service opt\-out policy**\.

   The console displays the effective policy applied to the selected account\.
**Note**  
You can't copy and paste an effective policy and use it as the JSON for another AI services opt\-out policy without significant changes\. AI services opt\-out policy documents must include the [inheritance operators](orgs_manage_policies_inheritance_mgmt.md#policy-operators) that specify how each setting is merged into the final effective policy\. 

------
#### [ New console ]

**To view the effective AI services opt\-out policy for an account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, choose the name of the account for which you want to view the effective AI services opt\-out policy\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the account that you want\.

1. On the **Policies** tab, in the **AI services opt\-out policies** section, choose **View the effective AI policy for this AWS account**\.

   The console displays the effective policy applied to the specified account\.
**Note**  
You can't copy and paste an effective policy and use it as the JSON for another AI services opt\-out policy without significant changes\. AI services opt\-out policy documents must include the [inheritance operators](orgs_manage_policies_inheritance_mgmt.md#policy-operators) that specify how each setting is merged into the final effective policy\. 

------
#### [ AWS CLI & AWS SDKs ]

**To view the effective AI services opt\-out policy for an account**  
You can use one of the following to view the effective AI services opt\-out policy:
+ AWS CLI: [aws organizations describe\-effective\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-effective-policy.html)

  The following example shows the effective AI services opt\-out policy for an account\.

  ```
  $ aws organizations describe-effective-policy \
      --policy-type AISERVICES_OPT_OUT_POLICY \
      --target-id 123456789012
  {
      "EffectivePolicy": {
          "PolicyContent": "{\"services\":{\"comprehend\":{\"opt_out_policy\":\"optOut\"},   ....TRUNCATED FOR BREVITY....   "opt_out_policy\":\"optIn\"}}}",
          "LastUpdatedTimestamp": "2020-12-09T12:58:53.548000-08:00",
          "TargetId": "123456789012",
          "PolicyType": "AISERVICES_OPT_OUT_POLICY"
      }
  }
  ```
+ AWS SDKs: [DescribeEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeEffectivePolicy.html) 

------