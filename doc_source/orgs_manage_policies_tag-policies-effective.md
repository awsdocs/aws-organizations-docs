# Viewing effective tag policies<a name="orgs_manage_policies_tag-policies-effective"></a>

Before you start checking compliance status for tagged resources in an account, it's helpful to first determine the effective tag policy for an account\.

## What is the effective tag policy?<a name="effective-tag-policy-defined"></a>

The *effective tag policy* specifies the tagging rules that apply to an account\. It is the aggregation of any tag policies the account inherits, plus any tag policy directly attached to the account\. When you attach a tag policy to the organization root, it applies to all accounts in your organization\. When you attach a tag policy to an OU, it applies to all accounts and OUs that belong to the OU\. 

For example, the tag policy attached to the organization root may define a `CostCenter` tag with five compliant values\. A separate tag policy attached to the account may restrict the `CostCenter` key to only two of the four compliant values\. The combination of these tag policies comprises the effective tag policy\. The result is that only two of the four compliant tag values defined in the organization root tag policy are compliant for the account\.

For more information and more advanced examples of how effective tag policies are generated, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.

## How to view the effective tag policy<a name="how-to-view-effective-tag-policy"></a>

You can view the effective tag policy for an account from the AWS Management Console, AWS API, or AWS Command Line Interface\.

**Minimum permissions**  
To view the effective tag policy for an account, you must have permission to run the following actions:  
`organizations:DescribeEffectivePolicy`
`organizations:DescribeOrganization`

------
#### [ AWS Management Console ]

**To view the effective tag policy for an account**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, choose the name of the account for which you want to view the effective tag policy\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\) to find the account that you want\.

1. On the **Policies** tab, in the **Tag policies** section, choose **View the effective tag policy for this AWS account**\.

   The console displays the effective policy applied to the specified account\.
**Note**  
You can't copy and paste an effective policy and use it as the JSON for another tag policy without significant changes\. Tag policy documents must include the [inheritance operators](orgs_manage_policies_inheritance_mgmt.md#policy-operators) that specify how each setting is merged into the final effective policy\. 

------
#### [ AWS CLI & AWS SDKs ]

**To view the effective tag policy for an account**  
You can use one of the following to view the effective tag policy:
+ AWS CLI: [describe\-effective\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-effective-policy.html)

  To determine what tagging rules are inherited by or attached to an account, run the following from the account and save the results to a file:

  ```
  $ aws organizations describe-effective-policy \
      --policy-type TAG_POLICY
  {
      "EffectivePolicy": {
          "PolicyContent": "{\"tags\":{\"costcenter\":{\"tag_value\":[\"*\"],\"tag_key\":\"CostCenter\"}}}",
          "LastUpdatedTimestamp": "2020-06-09T08:34:25.103000-07:00",
          "TargetId": "123456789012",
          "PolicyType": "TAG_POLICY"
      }
  }
  ```

  If a tag policy is attached to the account as well as to the root or any OUs, the combination of all of the inherited policies defines the account's effective tag policy\. In these cases, running `describe-effective-policy` from the account returns the merged content of all tag policies in the account's hierarchy\. 
+ AWS SDKs: [DescribeEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribeEffectivePolicy.html)

------