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
+ Management account: `CloudFormationStackSetsOrgAdmin`
+ Member accounts: `CloudFormationStackSetsOrgMember`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-cloudformation"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS CloudFormation Stacksets grant access to the following service principals:
+ Management account: `stacksets.cloudformation.amazonaws.com`
+ Member accounts: `member.org.stacksets.cloudformation.amazonaws.com`

## Enabling trusted access with AWS CloudFormation Stacksets<a name="integrate-enable-ta-cloudformation"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS CloudFormation StackSets console or the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by using the AWS CloudFormation Stacksets console\. This enables AWS CloudFormation Stacksets to perform required setup tasks\.

**To enable trusted access using the AWS CloudFormation Stacksets console**  
See [Enable Trusted Access with AWS Organizations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-enable-trusted-access.html) in the AWS CloudFormation User Guide\.

On the Organizations side, you can enable trusted access by using the AWS Organizations console

**Important**  
We recommend that where possible, you use the AWS CloudFormation StackSets console or tools to enable integration with Organizations so that AWS CloudFormation StackSets can perform any configuration that it requires\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS CloudFormation StackSets\.  
If you enable trusted access by using the tools provided by AWS CloudFormation StackSets then you don’t need to complete these steps\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. 

1. In the **Integrated services** section, find the row for **AWS CloudFormation StackSets**, choose the service’s name, and then choose Enable trusted access\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now enable that service using its console to work with AWS Organizations\.

------

## Disabling trusted access with AWS CloudFormation Stacksets<a name="integrate-disable-ta-cloudformation"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using the AWS Organizations console\. If you disable trusted access with AWS Organizations while you are using AWS CloudFormation StackSets, all previously created stack instances are retained\. However, stack sets deployed using the service\-linked role's permissions can no longer perform deployments to accounts managed by AWS Organizations\.

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS CloudFormation StackSets console or tools to disable integration with Organizations so that AWS CloudFormation StackSets can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS CloudFormation StackSets, wait until the administrator of AWS CloudFormation StackSets tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. 

1. In the **Integrated services** section, find the row for **AWS CloudFormation StackSets** and then choose the service’s name\.

1. In the confirmation dialog box, check the box for **Show the option to disable trusted access**, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI & AWS SDKs ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following commands to disable AWS CloudFormation StackSets as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle stacksets.cloudformation.amazonaws.com
  $ aws organizations disable-aws-service-access \
      --service-principle member.org.stacksets.cloudformation.amazonaws.com
  ```

  These commands produce no output when successful\.
+ AWS SDKs: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------