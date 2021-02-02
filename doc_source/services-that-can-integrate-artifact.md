# AWS Artifact and AWS Organizations<a name="services-that-can-integrate-artifact"></a>

AWS Artifact is a service that allows you to download AWS security compliance reports such as ISO and PCI reports\. Using AWS Artifact, a user in the organization's management account can automatically accept agreements on behalf of all member accounts in an organization, even as new reports and accounts are added\. Member account users can view and download agreements\. For more information, see [Managing an agreement for multiple accounts in AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/manage-org-agreement.html) in the *AWS Artifact User Guide*\.

Use the following information to help you to help you integrate AWS Artifact with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-artifact"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow AWS Artifact to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between AWS Artifact and Organizations, or if you remove the member account from the organization\.
+ `AWSArtifactAccountSync`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-artifact"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS Artifact grant access to the following service principals:
+ `AWSArtifactAccountSync`

## Enabling trusted access with AWS Artifact<a name="integrate-enable-ta-artifact"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using only the Organizations console or tools\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Artifact**, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, choose **Enable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Artifact that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ New console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Artifact**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Artifact that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Artifact as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal AWSArtifactAccountSync
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS Artifact<a name="integrate-disable-ta-artifact"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with AWS Artifact\.

You can disable trusted access using only the Organizations console or tools\.

AWS Artifact requires trusted access with AWS Organizations to work with organization agreements\. If you disable trusted access using AWS Organizations while you are using AWS Artifact for organization agreements, it stops functioning because it cannot access the organization\. Any organization agreements that you accept in AWS Artifact remain, but can't be accessed by AWS Artifact\. The AWS Artifact role that AWS Artifact creates remains\. If you then re\-enable trusted access, AWS Artifact continues to operate as before, without the need for you to reconfigure the service\. 

A standalone account that is removed from an organization no longer has access to any organization agreements\.

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS Artifact** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Artifact that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Artifact** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Artifact that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Artifact as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal AWSArtifactAccountSync
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------