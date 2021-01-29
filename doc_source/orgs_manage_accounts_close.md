# Closing an AWS account<a name="orgs_manage_accounts_close"></a>

**This topic applies to ***only*** AWS accounts that are managed by the AWS Organizations service**  
If you want to close an **Amazon\.com shopping account**, see [https://www\.amazon\.com/gp/help/customer/display\.html?nodeId=GDK92DNLSGWTV6MP](https://www.amazon.com/gp/help/customer/display.html?nodeId=GDK92DNLSGWTV6MP)\.
If you want to close an Amazon Web Services \(AWS\) account that is not part of an AWS Organization, see [https://aws\.amazon\.com/premiumsupport/knowledge\-center/close\-aws\-account/](https://aws.amazon.com/premiumsupport/knowledge-center/close-aws-account/)\.

**Note**  
AWS Organizations is introducing a new version of the Organizations management console\. You can switch between the old console and the new console by choosing the link in the notice boxes at the top of the console\. We encourage you to try the new version and let us know what you think\. We want your feedback and read each submission\.

If you no longer need a member account in your organization, and want to ensure that no one can accrue charges for it, you can close the account\. 

Before closing your account, back up any applications and data that you want to retain\. All resources and data that were stored in the account are lost and cannot be recovered\. For more information, see the KB article [ "How do I close my Amazon Web Services account?"](https://aws.amazon.com/premiumsupport/knowledge-center/close-aws-account/)

Immediately, the account can no longer be used for any AWS activity other than signing in as the root user to view past bills or to contact AWS Support\. For more information, see [Contacting Customer Support About Your Bill](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-get-answers.html)\.

**Important**  
Accounts that are closed for 90 days or more are no longer eligible for reinstatement\. At this point, any resources that were in the account can’t be recovered\.
Closed accounts are visible in your organization with the "suspended" state\. After an account has been in the suspended state for 90 days, it's no longer visible in your organization\.
You can close an account only by using the Billing and Cost Management console, not by using the AWS Organizations console or its tools\.
To close the management account for the organization, you must first either [remove ](orgs_manage_accounts_remove.md#orgs_manage_accounts_remove-member-account) or close all member accounts in the organization\. This is because closing the management account automatically deletes the organization, which succeeds only if there are no member accounts in the organization\.

You can close an account only by using the Billing and Cost Management console, not by using the AWS Organizations console or its tools\.

To close a management account, you must first [delete the organization](orgs_manage_org_delete.md), and then you can close it using the steps in the following procedure\.

------
#### [ Console ]

**To close an AWS account**

***Recommended:*** Before closing your account, back up any applications and data that you want to retain\. AWS can't recover or restore your account resources and data after your account is closed\. 

1. [Sign in as the root user of the account](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html) that you want to close, using the email address and password that are associated with the account\. If you sign in as an IAM user or role, you can't close an account\.
**Note**  
By default, member accounts that you create with AWS Organizations don't have a password that's associated with the account's root user\. To sign in, you must request a password for the root user\. For more information, see [Accessing a member account as the root user](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. Open the [Billing and Cost Management console](https://console.aws.amazon.com/billing)\.

1. On the navigation bar in the upper\-right corner, choose your account name \(or alias\) and then choose **My Account**\.

1. On the **Account Settings** page, scroll to the end of the page to the **Close Account** section\. Read and ensure that you understand the text next to the check box\.

1. Select the check box to confirm your understanding of the terms and then choose **Close Account**\.

1. In the confirmation box, choose **Close Account**\.

After you close an AWS account, you can no longer use it to access AWS services or resources\. For the 90 days after you close your account \(the "Post\-Closure Period"\), you will be able to log in to view past bills and access AWS Support\. You can contact AWS Support within the Post\-Closure Period to reopen the account\. For more information, see [How do I reopen my closed AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/reopen-aws-account/) in the Knowledge Center\.

**Note**  
For security reasons, AWS ***never*** reuses an AWS account ID number after the account is closed\. As a result of this security measure, if you happen to leave an ID for a closed account in an IAM policy, there could never be an unknown account with the same ID that would have access to your resources\. However, we recommend that you remove references to closed accounts, in accordance with the [security best practice of granting the least privilege needed to get the job done](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)\.

------