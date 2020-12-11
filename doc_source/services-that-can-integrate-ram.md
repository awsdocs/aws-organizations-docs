# AWS Resource Access Manager and AWS Organizations<a name="services-that-can-integrate-ram"></a>

AWS Resource Access Manager \(AWS RAM\) enables you to share specified AWS resources that you own with other AWS accounts\. It's a centralized service that provides a consistent experience for sharing different types of AWS resources across multiple accounts\.

For more information about AWS RAM, see the [https://docs.aws.amazon.com/ram/latest/userguide/what-is.html](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html)\.

Use the following information to help you to help you integrate AWS Resource Access Manager with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-ram)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-ram)
+ [Enabling trusted access with AWS RAM](#integrate-enable-ta-ram)
+ [Disabling trusted access with AWS RAM](#integrate-disable-ta-ram)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-ram"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS RAM to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS RAM and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleForResourceAccessManager`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-ram"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS RAM grant access to the following service principals:
+ `ram.amazonaws.com`

## Enabling trusted access with AWS RAM<a name="integrate-enable-ta-ram"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Resource Access Manager console or the AWS Organizations console\.

**To enable trusted access using the AWS RAM console or CLI**  
See [Enable Sharing with AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html#getting-started-sharing-orgs) in the *AWS RAM User Guide*\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS Resource Access Manager console or tools to enable integration with Organizations so that AWS Resource Access Manager can perform any configuration that it requires\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Resource Access Manager\.  
If you enable trusted access by using the tools provided by AWS Resource Access Manager then you don’t need to complete these steps\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **AWS Resource Access Manager**, choose the service’s name, and then choose Enable trusted access\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Resource Access Manager that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Resource Access Manager as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle ram.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS RAM<a name="integrate-disable-ta-ram"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We recommend that where possible, you use the AWS Resource Access Manager console or tools to disable integration with Organizations so that AWS Resource Access Manager can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Resource Access Manager, wait until the administrator of AWS Resource Access Manager tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **AWS Resource Access Manager** and then choose the service’s name\.

1. In the confirmation dialog box, check the box for **Show the option to disable trusted access**, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Resource Access Manager that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Resource Access Manager as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle ram.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------