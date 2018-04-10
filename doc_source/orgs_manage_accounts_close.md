# Closing an AWS Account<a name="orgs_manage_accounts_close"></a>

If you no longer need an AWS account \(whether a member in an organization or not\) and want to ensure that no one can accrue charges for it, you can close the account\. 

Before closing your account, back up any applications and data that you want to retain\. All resources and data that were stored in the account are lost and cannot be recovered\. For more information, see the KB article [ "How do I close my Amazon Web Services account?"](https://aws.amazon.com/premiumsupport/knowledge-center/close-aws-account/)

The account is not actually deleted, but it can no longer be used for any AWS activity other than signing in as the root user to view past bills or to contact AWS Customer Support\. For more information, see [Contacting Customer Support About Your Bill](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-get-answers.html)\.

**Important**  
Closing an account doesn't remove it from an organization\. A closed member account in an organization still counts towards your limit of accounts in the organization\. Before closing it, you can [remove the account from the organization](orgs_manage_accounts_remove.md) to avoid it counting against the limit\.

You can close an account only by using the Billing and Cost Management console, not by using the AWS Organizations console or its tools\. Use the following procedure to close an account\. 

**To close an AWS account \(console\)**

***Recommended: ***Before closing your account, back up any applications and data that you want to retain\. AWS can't recover or restore your account resources and data after your account is closed\. 

1. [Sign in as the root user of the account](http://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html) that you want to close, using the email address and password that are associated with the account\. If you sign in as an IAM user or role, you can't close an account\.
**Note**  
By default, member accounts that you create with Organizations do not have a password that is associated with the account's root user\. To sign in, you must request a password for the root user\. For more information, see [Accessing a Member Account as the Root User](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

1. Open the Billing and Cost Management console at [https://console.aws.amazon.com/billing/home#/](https://console.aws.amazon.com/billing/home#/)\.

1. On the navigation bar in the upper\-right corner, choose your account name \(or alias\), and then choose **My Account**\.

1. On the **Account Settings** page, scroll to the end of the page to the **Close Account** section\. Read and ensure that you understand the text next to the check box\.

1. Select the check box to confirm your understanding of the terms, and then choose **Close Account**\.

1. In the confirmation box, choose **Close Account**\.

After you close an AWS account, you can no longer use it to access AWS services or resources\. You can only view the account's past bills and access AWS Customer Support\. You can reopen the account by contacting AWS Support\. For more information, see [How do I reopen my closed AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/reopen-aws-account/) in the Knowledge Center\.