# AWS Directory Service and AWS Organizations<a name="services-that-can-integrate-directory-service"></a>

AWS Directory Service for Microsoft Active Directory, or AWS Managed Microsoft AD, lets you run Microsoft Active Directory \(AD\) as a managed service\. AWS Directory Service makes it easy to set up and run directories in the AWS Cloud or connect your AWS resources with an existing on\-premises Microsoft Active Directory\. AWS Managed Microsoft AD also integrates tightly with AWS Organizations to allow seamless directory sharing across multiple AWS accounts and any VPC in a Region\. For more information, see the [AWS Directory Service Administration Guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Use the following information to help you to help you integrate AWS Directory Service with AWS Organizations\.

**Topics**
+ [Enabling trusted access with AWS Directory Service](#integrate-enable-ta-directory-service)
+ [Disabling trusted access with AWS Directory Service](#integrate-disable-ta-directory-service)

## Enabling trusted access with AWS Directory Service<a name="integrate-enable-ta-directory-service"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Directory Service console or the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by using the AWS Directory Service console\. This enables AWS Directory Service to perform required setup tasks\.

**To enable trusted access using the AWS Directory Service console**  
To share a directory, which automatically enables trusted access, see [Share Your Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_directory_sharing.html) in the *AWS Directory Service Administration Guide*\. For step\-by\-step instructions, see [Tutorial: Sharing Your AWS Managed Microsoft AD Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_tutorial_directory_sharing.html)\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

**Important**  
We strongly recommend that where possible, you use the AWS Directory Service console or tools to enable integration with Organizations so that AWS Directory Service can perform any configuration that it requires\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Directory Service\.  
If you enable trusted access by using the tools provided by AWS Directory Service then you don’t need to complete these steps\.

------
#### [ Old console ]

**To enable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Directory Service**, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, choose **Enable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Directory Service that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ New console ]

**To enable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Directory Service**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Directory Service that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Directory Service as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Directory Service<a name="integrate-disable-ta-directory-service"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

 If you disable trusted access using AWS Organizations while you are using AWS Directory Service, all previously shared directories continue to operate as normal\. However, you can no longer share new directories within the organization until you enable trusted access again\.

On the Organizations side, you can disable trusted access by using the AWS Organizations console\.

**Important**  
We strongly recommend that where possible, you use the AWS Directory Service console or tools to disable integration with Organizations so that AWS Directory Service can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Directory Service, wait until the administrator of AWS Directory Service tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ Old console ]

**To disable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Directory Service** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Directory Service that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Directory Service** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Directory Service that they can now disable that service using its console or tools from working with AWS Organizations\.

------