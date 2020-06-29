# Closing an AWS account<a name="orgs_manage_accounts_close"></a>

If you no longer need an AWS account \(whether a member in an organization or not\) and want to ensure that no one can accrue charges for it, you can close the account\. 

Before closing your account, back up any applications and data that you want to retain\. All resources and data that were stored in the account are lost and cannot be recovered\. For more information, see the KB article [ "How do I close my Amazon Web Services account?"](https://aws.amazon.com/premiumsupport/knowledge-center/close-aws-account/)

Immediately, the account can no longer be used for any AWS activity other than signing in as the root user to view past bills or to contact AWS Support\. For more information, see [Contacting Customer Support About Your Bill](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-get-answers.html)\.

**Important**  
Accounts that are closed for 90 days are subject to permanent deletion after which the account and its resources can’t be recovered\.
Closed accounts are visible in your organization with the "suspended" state\. After an account is permanently deleted, it's no longer visible in your organization\.

You can close an account only by using the Billing and Cost Management console, not by using the AWS Organizations console or its tools\.

To close a master account, first [delete the organization](orgs_manage_org_delete.md), and then close it using the steps in the following procedure\.

**To close an AWS account \(console\)**

***Recommended:*** Before closing your account, back up any applications and data that you want to retain\. AWS can't recover or restore your account resources and data after your account is closed\. 

1. [Sign in as the root user of the account](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html) that you want to close, using the email address and password that are associated with the account\. If you sign in as an IAM user or role, you can't close an account\.
**Note**  
By default, member accounts that you create with AWS Organizations don't have a password that's associated with the account's root user\. To sign in, you must request a password for the root user\. For more information, see [Accessing a member account as the root user](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. Open the Billing and Cost Management console at [https://console.aws.amazon.com/billing/home#/](https://console.aws.amazon.com/billing/home#/)\.

1. On the navigation bar in the upper\-right corner, choose your account name \(or alias\) and then choose **My Account**\.

1. On the **Account Settings** page, scroll to the end of the page to the **Close Account** section\. Read and ensure that you understand the text next to the check box\.

1. Select the check box to confirm your understanding of the terms and then choose **Close Account**\.

1. In the confirmation box, choose **Close Account**\.

After you close an AWS account, you can no longer use it to access AWS services or resources\. For the 90 days after you close your account \(the "Post\-Closure Period"\), you will be able to log in to view past bills and access AWS Support\. You can contact AWS Support within the Post\-Closure Period to reopen the account\. For more information, see [How do I reopen my closed AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/reopen-aws-account/) in the Knowledge Center\.

**Note**  
For security reasons, AWS ***never*** reuses an AWS account ID number after the account is closed\. As a result of this security measure, if you happen to leave an ID for a closed account in an IAM policy, an unknown account with the same ID won't have access to your resources\. However, we recommend that you remove references to closed accounts, in accordance with the [security best practice of granting the least privilege needed to get the job done](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)\.