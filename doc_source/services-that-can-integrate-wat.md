# AWS Well\-Architected Tool and AWS Organizations<a name="services-that-can-integrate-wat"></a>

The AWS Well\-Architected Tool helps you document the state of your workloads and compares them to the latest AWS architectural best practices\.

Using AWS Well\-Architected Tool with Organizations enables both AWS Well\-Architected Tool and Organizations customers to simplify the process of sharing AWS Well\-Architected Tool resources with other members of their organization\.

For more information, see [Sharing your AWS Well\-Architected Tool resources](https://docs.aws.amazon.com/wellarchitected/latest/userguide/sharing.html) in the *AWS Well\-Architected Tool User Guide*\.

Use the following information to help you integrate AWS Well\-Architected Tool with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-wat"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows AWS WA Tool to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between AWS WA Tool and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForWellArchitected`

The service role policy is `AWSWellArchitectedOrganizationsServiceRolePolicy`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-wat"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS WA Tool grant access to the following service principals:
+ `wellarchitected.amazonaws.com`

## Enabling trusted access with AWS WA Tool<a name="integrate-enable-ta-wat"></a>

Allows the updating of AWS WA Tool to reflect hierarchical changes in an organization\.

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS Well\-Architected Tool console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Well\-Architected Tool console or tools to enable integration with Organizations\. This lets AWS Well\-Architected Tool perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS Well\-Architected Tool\. For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS Well\-Architected Tool console or tools then you don’t need to complete these steps\.

**To enable trusted access using the AWS WA Tool console**  
See [Sharing your AWS Well\-Architected Tool resources](https://docs.aws.amazon.com/wellarchitected/latest/userguide/sharing.html) in the *AWS Well\-Architected Tool User Guide*\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS Well\-Architected Tool**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS Well\-Architected Tool that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS Well\-Architected Tool as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal wellarchitected.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with AWS WA Tool<a name="integrate-disable-ta-wat"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using either the AWS Well\-Architected Tool or AWS Organizations tools\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Well\-Architected Tool console or tools to disable integration with Organizations\. This lets AWS Well\-Architected Tool perform any clean up that it requires, such as deleting resources or access roles that are no longer needed by the service\. Proceed with these steps only if you can’t disable integration using the tools provided by AWS Well\-Architected Tool\.  
If you disable trusted access by using the AWS Well\-Architected Tool console or tools then you don’t need to complete these steps\.

**To disable trusted access using the AWS WA Tool console**  
See [Sharing your AWS Well\-Architected Tool resources](https://docs.aws.amazon.com/wellarchitected/latest/userguide/sharing.html) in the *AWS Well\-Architected Tool User Guide*\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Well\-Architected Tool as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal wellarchitected.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------