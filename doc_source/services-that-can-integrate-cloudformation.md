# AWS CloudFormation StackSets and AWS Organizations<a name="services-that-can-integrate-cloudformation"></a>

AWS CloudFormation StackSets enables you to create, update, or delete stacks across multiple AWS accounts and AWS Regions with a single operation\. StackSets integration with AWS Organizations enables you to create stack sets with service\-managed permissions, using a service\-linked role that has the relevant permission in each member account\. This lets you deploy stack instances to member accounts in your organization\. You don't have to create the necessary AWS Identity and Access Management roles; StackSets creates the IAM role in each member account on your behalf\. You can also choose to enable automatic deployments to accounts that are added to your organization in the future\.

With trusted access between StackSets and Organizations enabled, the management account has permissions to create and manage stack sets for your organization\. The management account can register up to five member accounts as delegated administrators\. With trusted access enabled, delegated administrators also have permissions to create and manage stack sets for your organization\. Stack sets with service\-managed permissions are created in the management account, including stack sets that are created by delegated administrators\.

**Important**  
Delegated administrators have full permissions to deploy to accounts in your organization\. The management account cannot limit delegated administrator permissions to deploy to specific OUs or to perform specific stack set operations\.

 For more information about integrating StackSets with Organizations, see [Working with AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) in the *AWS CloudFormation User Guide*\.

Use the following information to help you to help you integrate AWS CloudFormation StackSets with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-cloudformation"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS CloudFormation Stacksets to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS CloudFormation Stacksets and Organizations, or if you remove the member account from the organization\.
+ Management account: `CloudFormationStackSetsOrgAdmin`
+ Member accounts: `CloudFormationStackSetsOrgMember`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-cloudformation"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS CloudFormation Stacksets grant access to the following service principals:
+ Management account: `stacksets.cloudformation.amazonaws.com`

  You can modify or delete this role only if you disabled trusted access between StackSets and Organizations\.
+ Member accounts: `member.org.stacksets.cloudformation.amazonaws.com`

  You can modify or delete this role from an account only if you first disable trusted access between StackSets and Organizations, or if you first remove the account from the target organization or organizational unit \(OU\)\.

## Enabling trusted access with AWS CloudFormation Stacksets<a name="integrate-enable-ta-cloudformation"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

Only an administrator in the Organizations management account has permissions to enable trusted access with another AWS service\. You can enable trusted access using either the AWS CloudFormation console or the Organizations console\.

You can enable trusted access using either the AWS CloudFormation StackSets console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS CloudFormation StackSets console or tools to enable integration with Organizations\. This lets AWS CloudFormation StackSets perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS CloudFormation StackSets\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS CloudFormation StackSets console or tools then you don’t need to complete these steps\.

**To enable trusted access using the AWS CloudFormation Stacksets console**  
See [Enable Trusted Access with AWS Organizations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-enable-trusted-access.html) in the AWS CloudFormation User Guide\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS CloudFormation StackSets**, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, choose **Enable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ New console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS CloudFormation StackSets**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS CloudFormation StackSets as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal stacksets.cloudformation.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS CloudFormation Stacksets<a name="integrate-disable-ta-cloudformation"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in an Organizations management account has permissions to disable trusted access with another AWS service\. You can disable trusted access only by using the Organizations console\. If you disable trusted access with Organizations while you are using StackSets, all previously created stack instances are retained\. However, stack sets deployed using the service\-linked role's permissions can no longer perform deployments to accounts managed by Organizations\. Before you can disable trusted access with AWS Organizations, you must deregister all delegated administrators\. For details, see [Register a delegated administrator](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-delegated-admin.html)\.

You can disable trusted access using only the Organizations console or tools\.

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS CloudFormation StackSets** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS CloudFormation StackSets** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS CloudFormation StackSets as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal stacksets.cloudformation.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for AWS CloudFormation Stacksets<a name="integrate-enable-da-cloudformation"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for AWS CloudFormation Stacksets that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of AWS CloudFormation Stacksets\.

See [Register a delegated administrator](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-delegated-admin.html) in the *AWS CloudFormation User Guide*\.