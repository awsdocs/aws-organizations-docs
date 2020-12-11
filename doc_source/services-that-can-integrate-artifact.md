# AWS Artifact and AWS Organizations<a name="services-that-can-integrate-artifact"></a>

AWS Artifact is a service that allows you to download AWS security compliance reports such as ISO and PCI reports\. Using AWS Artifact, a user in the organization's management account can automatically accept agreements on behalf of all member accounts in an organization, even as new reports and accounts are added\. Member account users can view and download agreements\. For more information, see [Managing an agreement for multiple accounts in AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/manage-org-agreement.html) in the *AWS Artifact User Guide*\.

Use the following information to help you to help you integrate AWS Artifact with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-artifact)
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-artifact)
+ [Enabling trusted access with AWS Artifact](#integrate-enable-ta-artifact)
+ [Disabling trusted access with AWS Artifact](#integrate-disable-ta-artifact)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-artifact"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Artifact to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Artifact and Organizations or if the account is removed from the organization\.
+ `AWSArtifactAccountSync`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-artifact"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked role used by AWS Artifact grants access to the following service principal:
+ `AWSArtifactAccountSync`

## Enabling trusted access with AWS Artifact<a name="integrate-enable-ta-artifact"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

On the Organizations side, you can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. In the **Trusted access for AWS services** section, find the row for **AWS Artifact** and then choose **Enable access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Artifact that they can now enable that service to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Artifact as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principle AWSArtifactAccountSync
  ```

  The previous command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Artifact<a name="integrate-disable-ta-artifact"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access only by using the AWS Organizations console\. AWS Artifact requires trusted access with AWS Organizations to work with organization agreements\. If you disable trusted access using AWS Organizations while you are using AWS Artifact for organization agreements, it stops functioning because it cannot access the organization\. Any organization agreements that you accept in AWS Artifact remain, but can't be accessed by AWS Artifact\. The AWS Artifact role that AWS Artifact creates remains\. If you then re\-enable trusted access, AWS Artifact continues to operate as before, without the need for you to reconfigure the service\. 

A standalone account that is removed from an organization no longer has access to any organization agreements\.

On the Organizations side, you can disable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's management account\. 

1. In the upper\-right corner, choose **Settings**\.

1. If you are the administrator of only AWS Organizations and not AWS Artifact, wait until the administrator of AWS Artifact tells you that they disabled integration with that service's console or tools, and that any resources have been cleaned up\.

1. In the **Trusted access for AWS services** section, find the entry for **AWS Artifact**, and then choose **Disable access**\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using an Organizations AWS CLI command or API**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Artifact as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principle AWSArtifactAccountSync
  ```

  The previous command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------