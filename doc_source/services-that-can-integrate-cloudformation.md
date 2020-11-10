# AWS CloudFormation StackSets and AWS Organizations<a name="services-that-can-integrate-cloudformation"></a>

AWS CloudFormation StackSets enables you to create, update, or delete stacks across multiple accounts and Regions with a single operation\.

For more information about StackSets, see [Working with AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) in the *AWS CloudFormation User Guide*\.

Use the following information to help you to help you integrate AWS CloudFormation StackSets with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-cloudformation)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-cloudformation)
+ [Enabling trusted access with AWS CloudFormation Stacksets](#integrate-enable-ta-cloudformation)
+ [Disabling trusted access with AWS CloudFormation Stacksets](#integrate-disable-ta-cloudformation)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-cloudformation"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS CloudFormation Stacksets to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS CloudFormation Stacksets and Organizations or if the account is removed from the organization or target organizational unit\.
+ Management account:  `CloudFormationStackSetsOrgAdmin`
+ Member accounts: `CloudFormationStackSetsOrgMember`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-cloudformation"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS CloudFormation Stacksets grant access to the following service principals:
+ `stacksets.cloudformation.amazonaws.com`
+ `member.org.stacksets.cloudformation.amazonaws.com`

## Enabling trusted access with AWS CloudFormation Stacksets<a name="integrate-enable-ta-cloudformation"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS CloudFormation StackSets console or the AWS Organizations console\.

**To enable trusted access using the AWS CloudFormation Stacksets console**  
See [Enable Trusted Access with AWS Organizations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-enable-trusted-access.html) in the AWS CloudFormation User Guide\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find the row for **AWS CloudFormation StackSets** that you want and then choose **Enable access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now enable that service to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS CloudFormation Stacksets<a name="integrate-disable-ta-cloudformation"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using the AWS Organizations console\. If you disable trusted access with AWS Organizations while you are using AWS CloudFormation StackSets, all previously created stack instances are retained\. However, stack sets deployed using the service\-linked role's permissions can no longer perform deployments to accounts managed by AWS Organizations\.

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. If you are the administrator of only AWS Organizations and not AWS CloudFormation StackSets, wait until the administrator of AWS CloudFormation StackSets tells you that they disabled integration with that service's console or tools, and that any resources have been cleaned up\.

1. In the **Trusted access for AWS services** section, find the entry for **AWS CloudFormation StackSets**, and then choose **Disable access**\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------