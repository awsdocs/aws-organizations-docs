# Closing an AWS account<a name="orgs_manage_accounts_close"></a>

**This topic applies ***only*** to AWS accounts**  
To close an **Amazon\.com shopping account**, see [http://www\.amazon\.com/gp/help/customer/display\.html?nodeId=GDK92DNLSGWTV6MP](http://www.amazon.com/gp/help/customer/display.html?nodeId=GDK92DNLSGWTV6MP)\.

If you no longer need a member account in your organization, and want to ensure that no one can accrue charges for it, you can close the account from the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2) following the instructions in this section\. You can also close an AWS account from the AWS Billing Console\. To learn more, see [Closing an account](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/close-account.html) in the *AWS Billing User Guide*\.

Before closing your account, back up any applications and data that you want to retain\. Review [How do I check for active resources that I no longer need on my AWS account? ](https://aws.amazon.com/premiumsupport/knowledge-center/check-for-active-resources/) in the Knowledge Center\. 

Immediately, the account can no longer be used for any AWS activity other than signing in as the root user to view past bills or to contact AWS Support\. For more information, see [Contacting Customer Support About Your Bill](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-get-answers.html)\.

## Impacts of closing an account<a name="orgs_account_close_impacts"></a>

When you close an AWS account, there are some impacts that you should consider prior to account closure\. 
+ The root user's email address can't be reused if you close an account\.
+ To close the management account for the organization, you must first either [remove ](orgs_manage_accounts_remove.md#orgs_manage_accounts_remove-member-account) or close all member accounts in the organization\. By closing the management account, it automatically deletes the organization, as long as there are no member accounts in the organization\.
+ You can only close 10% of active member accounts within a rolling 30 day period\. This quota is not bound by a calendar month, but starts when you close an account\. Within 30 days of that initial account closure, you can't exceed the 10% account closure limit\. The maximum account closure is 200, even if 10% of active accounts exceeds 200\. For more information about Organizations quotas, see [Quotas for AWS Organizations](orgs_reference_limits.md)\.
+ If you use AWS Control Tower, you need to unmanage the accounts before you attempt to close an account\. See [ Unmanage a member account](https://docs.aws.amazon.com/controltower/latest/userguide/unmanage-account.html) in the * AWS Control Tower User Guide*\.
+ If you have an AWS account that is linked to a AWS GovCloud \(US\) account, you need to close the standard account before you close the AWS GovCloud \(US\) account\. To learn important pre\-closure details, see [ Closing an AWS GovCloud \(US\) account](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/Closing-govcloud-account.html) in the * AWS GovCloud \(US\) User Guide*\.

**Best practice we recommend but is not required for security:**

As a best practice, we recommend that you remove references from any IAM permissions or policies to closed accounts, in accordance with the [security best practice of granting the least privilege needed to get the job done](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)\. This is not a security issue because AWS never reuses an ID number after the account is closed\. IAM Access Analyzer will notify you if you have an ID for a closed account in an IAM policy\. 

**From the time you close the account until 90 days expire:**
+ Closed accounts are visible in your organization with the SUSPENDED state\. 
+ Some active resources that are not terminated prior to account closure can continue to incur fees if you decide to reopen the account within 90 days\. For more information, see [ How do I terminate active resources I no longer need on my AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/) in the Knowledge Center\. 
+ You will be able to log in to view past bills and access AWS Support\. 

**After the 90 day grace period expires:**
+ A closed AWS account is no longer visible in your organization\.
+ AWS accounts are no longer eligible for reinstatement\. At this point, any AWS resources that were in the account can’t be recovered\.

## Closing an AWS account<a name="orgs_account_close_proc"></a>

When you sign in to the organization's management account, you can close member accounts that are part of your organization\. To do this, complete the following steps\.

------
#### [ AWS Management Console ]

**To close an AWS account**
**Note**  
Optionally, you can close an AWS member account from the [Billing and Cost Management console](https://console.aws.amazon.com/billing)\. To learn more, see [Closing an account](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/close-account.html) in the *AWS Billing User Guide*\.

1.  Before closing your account, back up any applications and data that you want to retain\. Review [How do I check for active resources that I no longer need on my AWS account? ](https://aws.amazon.com/premiumsupport/knowledge-center/check-for-active-resources/) in the Knowledge Center\.

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, find and choose the name of the member account you want to close\. You can navigate the OU hierarchy, or look at a flat list of accounts without the OU structure\. 

1. Choose **Close** next to the account name at the top of the page\.

1. Select each check box to acknowledge all required account closure statements\. 

1. Enter the member account ID and then choose **Close Account**\. 

After you close an AWS account, you can no longer use it to access AWS services or resources\. You can contact AWS Support within the Post\-Closure Period to reopen the account\. For more information, see [How do I reopen my closed AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/reopen-aws-account/) in the Knowledge Center\.

------
#### [ AWS CLI & AWS SDKs ]

**To close an AWS account**  
You can use one of the following commands to close an AWS account:
+ AWS CLI: [close\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/close-account.html)

  ```
  $ aws organizations close-account \
      --account-id 123456789012
  ```

  This command produces no output when successful\.
+ AWS SDKs: [CloseAccount](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CloseAccount.html)

------

## Protecting accounts from closure<a name="orgs_account_close_policy"></a>

If you want to protect an AWS account from accidental closure, you can create an IAM policy to specify which accounts are exempt from closure\. Any member account protected with these policies can’t be closed\. This can't be accomplished with an SCP, because they don't affect principals in the management account\.

You can create an IAM policy that denies closing accounts in either of two ways:
+ Explicitly list each account that you want to protect in the policy by including the `arn` in the `Resource` element\. To see an example, see [ Prevent accounts listed in this policy from getting closed](#example_policy_prevent_close_account_arn)\.
+ Tag individual accounts to prevent them from getting closed\. Use the `aws:ResourceTag` tag global condition key in your policy to prevent any account with the tag from being closed\. To learn how to tag an account, see [Tagging Organizations resources](orgs_tagging.md)\. To see an example, see [Prevent accounts with tag from getting closed ](#example_policy_tag_close_account)\.

### Example IAM policies that prevent AWS account closures<a name="orgs_close_account_policy_examples"></a>

****Examples in this category****
+ [Prevent accounts with tag from getting closed](#example_policy_tag_close_account)
+ [Prevent accounts listed in this policy from getting closed](#example_policy_prevent_close_account_arn)

#### Prevent accounts with tag from getting closed<a name="example_policy_tag_close_account"></a>

You can attach the following policy to an identity in your management account\. This policy prevents principals in the management account from closing any member account that is tagged with the `aws:ResourceTag` tag global condition key, the `AccountType` key and the `Critical` tag value\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PreventCloseAccountForTaggedAccts",
            "Effect": "Deny",
            "Action": "organizations:CloseAccount",
            "Resource": "*",
            "Condition": {
                "StringEquals": {"aws:ResourceTag/AccountType": "Critical"}
            }
        }
    ]
}
```

#### Prevent accounts listed in this policy from getting closed<a name="example_policy_prevent_close_account_arn"></a>

You can attach the following policy to an identity in your management account\. This policy prevents principals in the management account from closing accounts explicitly specified in the `Resource` element\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PreventCloseAccount",
            "Effect": "Deny",
            "Action": "organizations:CloseAccount",
            "Resource": [
                "arn:aws:organizations::555555555555:account/o-12345abcdef/123456789012",
                "arn:aws:organizations::555555555555:account/o-12345abcdef/123456789014"
            ]
        }
    ]
}
```