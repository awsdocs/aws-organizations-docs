# AWS Directory Service and AWS Organizations<a name="services-that-can-integrate-directory-service"></a>

AWS Directory Service for Microsoft Active Directory, or AWS Managed Microsoft AD, lets you run Microsoft Active Directory \(AD\) as a managed service\. AWS Directory Service makes it easy to set up and run directories in the AWS Cloud or connect your AWS resources with an existing on\-premises Microsoft Active Directory\. AWS Managed Microsoft AD also integrates tightly with AWS Organizations to allow seamless directory sharing across multiple AWS accounts and any VPC in a Region\. For more information, see the [AWS Directory Service Administration Guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Use the following information to help you to help you integrate AWS Directory Service with AWS Organizations\.

**Topics**
+ [Enabling trusted access with AWS Directory Service](#integrate-enable-ta-directory-service)
+ [Disabling trusted access with AWS Directory Service](#integrate-disable-ta-directory-service)

## Enabling trusted access with AWS Directory Service<a name="integrate-enable-ta-directory-service"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using only the AWS Directory Service console\.

**To enable trusted access using the AWS Directory Service console**  
To share a directory, which automatically enables trusted access, see [Share Your Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_directory_sharing.html) in the *AWS Directory Service Administration Guide*\. For step\-by\-step instructions, see [Tutorial: Sharing Your AWS Managed Microsoft AD Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_tutorial_directory_sharing.html)\.

## Disabling trusted access with AWS Directory Service<a name="integrate-disable-ta-directory-service"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

 If you disable trusted access using AWS Organizations while you are using AWS Directory Service, all previously shared directories continue to operate as normal\. However, you can no longer share new directories within the organization until you enable trusted access again\.

On the Organizations side, you can disable trusted access by using the AWS Organizations console\.

**Important**  
We recommend that where possible, you use the AWS Directory Service console or tools to disable integration with Organizations so that AWS Directory Service can perform any cleanup steps that it requires\. Proceed with these steps only if you can’t disable integration using the other service’s tools\.  
If you are the administrator of only AWS Organizations and not AWS Directory Service, wait until the administrator of AWS Directory Service tells you that they disabled integration with that service’s console or tools, and that any resources have been cleaned up\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Services](https://console.aws.amazon.com/organizations/home/services)** page in the console\.

1. In the **Integrated services** section, find the row for **AWS Directory Service** and then choose the service’s name\.

1. In the confirmation dialog box, check the box **Show the option to disable trusted access**, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Directory Service that they can now disable that service using its console or tools from working with AWS Organizations\.

------