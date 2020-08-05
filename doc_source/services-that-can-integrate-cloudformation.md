# AWS CloudFormation StackSets and AWS Organizations<a name="services-that-can-integrate-cloudformation"></a>

AWS CloudFormation StackSets enables you to create, update, or delete stacks across multiple accounts and Regions with a single operation\. For more information about StackSets, see [Working with AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) in the *AWS CloudFormation User Guide*\.

The following list provides information that you need when integrating AWS CloudFormation StackSets with AWS Organizations:
+ **To enable trusted access with AWS Organizations:** Only an administrator in an AWS Organizations master account has permissions to enable trusted access\. You can enable trusted access using either the AWS CloudFormation console or the AWS Organizations console\. To enable trusted access using the AWS CloudFormation console, see [Enable Trusted Access with AWS Organizations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-enable-trusted-access.html) in the AWS CloudFormation User Guide\. To enable trusted access using the AWS Organizations console:

  1. Sign in to AWS as an administrator of the master account and open the AWS Organizations console at [https://console.aws.amazon.com/organizations/](https://console.aws.amazon.com/organizations/)\.

  1. On the **Settings** page, next to **AWS CloudFormation**, choose **Enable access**\.
+ **To disable trusted access with AWS Organizations:** Only an administrator in an AWS Organizations master account has permissions to disable trusted access\. You can only disable trusted access using the AWS Organizations console\. If you disable trusted access with AWS Organizations while you are using AWS CloudFormation StackSets, all previously created stack instances are retained\. However, stack sets with service\-managed permissions will no longer perform deployments to accounts managed by AWS Organizations\.

  1. Sign in to AWS as an administrator of the master account and open the AWS Organizations console at [https://console.aws.amazon.com/organizations/](https://console.aws.amazon.com/organizations/)\.

  1. On the **Settings** page, next to **AWS CloudFormation**, choose **Disable access**\.
+ **Name suffixes of the IAM service\-linked roles that are automatically created in administrator and target accounts** when trusted access is enabled:
  + Administrator \(management\) account:  `CloudFormationStackSetsOrgAdmin`\. You can modify or delete this role only if trusted access with AWS Organizations is disabled\.
  + Target accounts: `CloudFormationStackSetsOrgMember`\. You can modify or delete this role only if trusted access with AWS Organizations is disabled, or if the account is removed from the target organization or organizational unit \(OU\)\.