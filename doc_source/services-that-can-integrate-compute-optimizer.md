# AWS Compute Optimizer and AWS Organizations<a name="services-that-can-integrate-compute-optimizer"></a>

AWS Compute Optimizer is a service that analyzes the configuration and utilization metrics of your AWS resources\. Resource examples include Amazon Elastic Compute Cloud \(Amazon EC2\) instances and Auto Scaling groups\. Compute Optimizer reports whether your resources are optimal and generates optimization recommendations to reduce the cost and improve the performance of your workloads\. For more information about Compute Optimizer, see the [AWS Compute Optimizer User Guide](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is.html)\.

Use the following information to help you integrate AWS Compute Optimizer with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-compute-optimizer"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows Compute Optimizer to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between Compute Optimizer and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForComputeOptimizer`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-compute-optimizer"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Compute Optimizer grant access to the following service principals:
+ `compute-optimizer.amazonaws.com`

## Enabling trusted access with Compute Optimizer<a name="integrate-enable-ta-compute-optimizer"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Compute Optimizer console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Compute Optimizer console or tools to enable integration with Organizations\. This lets AWS Compute Optimizer perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Compute Optimizer\. For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Compute Optimizer console or tools then you don’t need to complete these steps\.

**To enable trusted access using the Compute Optimizer console**  
You must sign in to the Compute Optimizer console using your organization's management account\. Opt\-in on behalf of your organization by following the instructions at [Opting in your Account](https://docs.aws.amazon.com/compute-optimizer/latest/ug/getting-started.html#account-opt-in) in the *AWS Compute Optimizer User Guide*\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Compute Optimizer**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Compute Optimizer that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Compute Optimizer as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal compute-optimizer.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with Compute Optimizer<a name="integrate-disable-ta-compute-optimizer"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with AWS Compute Optimizer\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Compute Optimizer as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal compute-optimizer.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for Compute Optimizer<a name="integrate-enable-da-compute-optimizer"></a>

When you designate a member account to be a delegated administrator for the organization, users and roles from the designated account can manage the AWS account metadata for other member accounts in the organization\. If you don't enable a delegated admin account, then these tasks can be performed only by the organization's management account\. This helps you to separate management of the organization from management of your account details\.

**Minimum permissions**  
Only an IAM user or role in the Organizations management account can configure a member account as a delegated administrator for Compute Optimizer in the organization

For instructions about enabling a delegated administrator account for Compute Optimizer, see [https://docs.aws.amazon.com/compute-optimizer/latest/ug/delegate-administrator-account.html](https://docs.aws.amazon.com/compute-optimizer/latest/ug/delegate-administrator-account.html) in the *AWS Compute Optimizer User Guide*\.

------
#### [ AWS CLI, AWS API ]

If you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, you can use the following commands:
+ AWS CLI: 

  ```
  $  aws organizations register-delegated-administrator \
      --account-id 123456789012 \
      --service-principal compute-optimizer.amazonaws.com
  ```
+ AWS SDK: Call the Organizations `RegisterDelegatedAdministrator` operation and the member account's ID number and identify the account service `principal account.amazonaws.com` as parameters\. 

------

## Disabling a delegated administrator for Compute Optimizer<a name="integrate-disable-da-compute-optimizer"></a>

Only an administrator in the organization management account can configure a delegated administrator for Compute Optimizer\.

 To disable the delegated admin Compute Optimizer account using the Compute Optimizer console, see [https://docs.aws.amazon.com/compute-optimizer/latest/ug/delegate-administrator-account.html](https://docs.aws.amazon.com/compute-optimizer/latest/ug/delegate-administrator-account.html) in the *AWS Compute Optimizer User Guide*\.

 To remove a delegated administrator using the AWS AWS CLI, see [deregister\-delegated\-administrator](https://docs.aws.amazon.com/cli/latest/reference/organizations/deregister-delegated-administrator.html) in the *AWS AWS CLI Command Reference*\.