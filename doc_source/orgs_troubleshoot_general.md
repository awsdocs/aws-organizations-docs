# Troubleshooting general issues<a name="orgs_troubleshoot_general"></a>

Use the information here to help you diagnose and fix access\-denied or other common issues that you might encounter when working with AWS Organizations\.

**Topics**
+ [I get an "access denied" message when I make a request to AWS Organizations](#troubleshoot_general_access-denied-service)
+ [I get an "access denied" message when I make a request with temporary security credentials](#troubleshoot_general_access-denied-temp-creds)
+ [I get an "access denied" message when I try to leave an organization as a member account or remove a member account as the master account](#troubleshoot_general_error-leaving-org)
+ [I get a "quota exceeded" message when I try to add an account to my organization](#troubleshoot_general_error-adding-account)
+ [I get a "this operation requires a wait period" message while adding or removing accounts](#troubleshoot_general_error-wait-req)
+ [I get an "organization is still initializing" message when I try to add an account to my organization](#troubleshoot_general_error-still-init)
+ [I get an "Invitations are disabled" message when I try to invite an account to my organization\.](#troubleshoot_general_error-changing-feature-set)
+ [I used an incorrect email address when I created a member account](#troubleshoot_incorrect-email)
+ [Changes that I make aren't always immediately visible](#troubleshoot_general_eventual-consistency)

## I get an "access denied" message when I make a request to AWS Organizations<a name="troubleshoot_general_access-denied-service"></a>
+ Verify that you have permissions to call the action and resource that you have requested\. An administrator must grant permissions by attaching an IAM policy to your IAM user or to a group that you're a member of\. If the policy statements that grant those permissions include any conditions, such as time\-of\-day or IP address restrictions, you also must meet those requirements when you send the request\. For information about viewing or modifying policies for an IAM user, group, or role, see [Working with Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html) in the *IAM User Guide*\.
+ If you are signing API requests manually \(without using the [AWS SDKs](http://aws.amazon.com/tools/)\), verify that you have correctly [signed the request](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)\.

## I get an "access denied" message when I make a request with temporary security credentials<a name="troubleshoot_general_access-denied-temp-creds"></a>
+ Verify that the IAM user or role that you are using to make the request has the correct permissions\. Permissions for temporary security credentials are derived from an IAM user or role, so the permissions are limited to those granted to the IAM user or role\. For more information about how permissions for temporary security credentials are determined, see [Controlling Permissions for Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access.html) in the *IAM User Guide*\.
+ Verify that your requests are being signed correctly and that the request is well formed\. For details, see the [toolkit](http://aws.amazon.com/tools/) documentation for your chosen SDK or [Using Temporary Security Credentials to Request Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html) in the *IAM User Guide*\.
+ Verify that your temporary security credentials haven't expired\. For more information, see [Requesting Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_request.html) in the *IAM User Guide*\. 

## I get an "access denied" message when I try to leave an organization as a member account or remove a member account as the master account<a name="troubleshoot_general_error-leaving-org"></a>
+ You can remove a member account only after you enable IAM user access to billing in the member account\. For more information, see [Activating Access to the Billing and Cost Management Console](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/grantaccess.html#ControllingAccessWebsite-Activate) in the *AWS Billing and Cost Management User Guide*\.
+ You can remove an account from your organization only if the account has the information required for it to operate as a standalone account\. When you create an account in an organization using the AWS Organizations console, API, or AWS CLI commands, that information isn't automatically collected\. For an account that you want to make standalone, you must accept the AWS Customer Agreement, choose a support plan, provide and verify the required contact information, and provide a current payment method\. AWS uses the payment method to charge for any billable \(not AWS Free Tier\) AWS activity that occurs while the account isn't attached to an organization\. For more information, see [Leaving an organization as a member account](orgs_manage_accounts_remove.md#orgs_manage_accounts_leave-as-member)\.

## I get a "quota exceeded" message when I try to add an account to my organization<a name="troubleshoot_general_error-adding-account"></a>

There is a maximum number of accounts that you can have in an organization\. Deleted or closed accounts continue to count against this quota\.

An invitation to join counts against the maximum number of accounts in your organization\. The count is returned if the invited account declines, the master account cancels the invitation, or the invitation expires\.
+ Before you close or delete an AWS account, [remove it from your organization](orgs_manage_accounts_remove.md) so that it doesn't continue to count against your quota\.
+ Contact [AWS Support](https://console.aws.amazon.com/support/home#/) to request a quota increase\.

## I get a "this operation requires a wait period" message while adding or removing accounts<a name="troubleshoot_general_error-wait-req"></a>

Some actions require a wait period\. For example, you can't immediately remove newly created accounts\. Try the action again later\. If you experience issues with account quotas while adding and removing accounts, contact [AWS Support](https://console.aws.amazon.com/support/home#/) to request a quota increase\.

## I get an "organization is still initializing" message when I try to add an account to my organization<a name="troubleshoot_general_error-still-init"></a>

If you receive this error and it's been over an hour since you created the organization, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

## I get an "Invitations are disabled" message when I try to invite an account to my organization\.<a name="troubleshoot_general_error-changing-feature-set"></a>

This happens when you [enable all features in your organization](orgs_manage_org_support-all-features.md)\. This operation can take some time and requires that all member accounts respond\. Until the operation is completed, you can't invite new accounts to join the organization\.

## I used an incorrect email address when I created a member account<a name="troubleshoot_incorrect-email"></a>

If you created a member account in an organization with an incorrect email address then you might not be able to sign in to the member account as the root user\. In that case, you can try to access the master account access role for the account\. For more information, see [Accessing a member account that has a master account access role](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)\. If that doesn't work then you can't correct the member account's email address yourself\. Instead, contact AWS Support to correct the email address on the member account\. Use your browser to access the [Contact Us](https://aws.amazon.com/contact-us/) page, and choose the item regarding Billing to contact AWS Support\.

## Changes that I make aren't always immediately visible<a name="troubleshoot_general_eventual-consistency"></a>

As a service that is accessed through computers in data centers around the world, AWS Organizations uses a distributed computing model called [eventual consistency](https://wikipedia.org/wiki/Eventual_consistency)\. Any change that you make in AWS Organizations takes time to become visible from all possible endpoints\. Some of the delay results from the time it takes to send the data from server to server or from replication zone to replication zone\. AWS Organizations also uses caching to improve performance, but in some cases this can add time\. The change might not be visible until the previously cached data times out\.

Design your global applications to account for these potential delays and ensure that they work as expected, even when a change made in one location isn't instantly visible at another\.

For more information about how some other AWS services are affected by this, consult the following resources:
+ [Managing Data Consistency](https://docs.aws.amazon.com/redshift/latest/dg/managing-data-consistency.html) in the *Amazon Redshift Database Developer Guide*
+ [Amazon S3 Data Consistency Model](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel) in the *Amazon Simple Storage Service Developer Guide*
+ [Ensuring Consistency When Using Amazon S3 and Amazon Elastic MapReduce for ETL Workflows](http://aws.amazon.com/blogs/big-data/ensuring-consistency-when-using-amazon-s3-and-amazon-elastic-mapreduce-for-etl-workflows/) in the AWS Big Data Blog
+ [EC2 Eventual Consistency](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#eventual-consistency) in the *Amazon EC2 API Reference*\.