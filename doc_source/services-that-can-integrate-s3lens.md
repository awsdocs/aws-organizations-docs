# Amazon S3 Storage Lens and AWS Organizations<a name="services-that-can-integrate-s3lens"></a>

By giving Amazon S3 Storage Lens trusted access to your organization, you allow it to collect and aggregate metrics across all of the AWS accounts in your organization\. S3 Storage Lens does this by accessing the list of accounts that belong to your organization and collects and analyzes the storage and usage and activity metrics for all of them\. 

For more information, see \. For more information, see the [Using service\-linked roles for Amazon S3 Storage Lens](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-service-linked-roles.html) in the *Amazon S3 Storage Lens User Guide*\. 

Use the following information to help you integrate Amazon S3 Storage Lens with AWS Organizations\.



## Service\-linked role created when you enable integration<a name="integrate-enable-slr-s3lens"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow Amazon S3 Storage Lens to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Amazon S3 Storage Lens and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForS3StorageLens` 

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-s3lens"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Amazon S3 Storage Lens grant access to the following service principals:
+ `storage-lens.s3.amazonaws.com`

## Enabling trusted access with Amazon S3 Storage Lens<a name="integrate-enable-ta-s3lens"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the Amazon S3 Storage Lens console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the Amazon S3 Storage Lens console or tools to enable integration with Organizations\. This lets Amazon S3 Storage Lens perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by Amazon S3 Storage Lens\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the Amazon S3 Storage Lens console or tools then you don’t need to complete these steps\.

**To enable trusted access using the Amazon S3 console**  
See [How to enable Trusted Access](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/storage_lens_console_organizations_enabling_trusted_access.html ) in the *Amazon Simple Storage Service Developer Guide*\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **Amazon S3 Storage Lens**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Amazon S3 Storage Lens that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable Amazon S3 Storage Lens as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal storage-lens.s3.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with Amazon S3 Storage Lens<a name="integrate-disable-ta-s3lens"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

You can disable trusted access using only the Amazon S3 Storage Lens tools\.

You can disable trusted access using the Amazon S3 console, the AWS CLI or any of the AWS SDKs\. 

**To disable trusted access using the Amazon S3 console**  
See [How to disable Trusted Access](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/storage_lens_console_organizations_disabling_trusted_access.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Enabling a delegated administrator account for Amazon S3 Storage Lens<a name="integrate-enable-da-s3lens"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for Amazon S3 Storage Lens that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Amazon S3 Storage Lens\.

**Minimum permissions**  
Only an IAM user or role in the Organizations management account with the following permission can configure a member account as a delegated administrator for Amazon S3 Storage Lens in the organization:  
`organizations:RegisterDelegatedAdministrator`  
`organizations:DeregisterDelegatedAdministrator`

Amazon S3 Storage Lens supports a maximum of 5 delegated administrator accounts in your organization\.

**To designate a member account as a delegated administrator for Amazon S3 Storage Lens**  
You can register a delegated administrator using the Amazon S3 console, the AWS CLI or any of the AWS SDKs\. To register a member account as a delegated administrator account for your organization using the Amazon S3 console, see [How to register a Delegated Administrator](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/storage_lens_console_organizations_registering_delegated_admins.html) in the *Amazon Simple Storage Service Developer Guide*\.

**To deregister a delegated administrator for Amazon S3 Storage Lens**  
You can deregister a delegated administrator using the Amazon S3 console, the AWS CLI or any of the AWS SDKs\. To deregister a delegated administrator using the Amazon S3 console, see [How to unregister a Delegated Administrator](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/storage_lens_console_organizations_deregistering_delegated_admins.html) in the *Amazon Simple Storage Service Developer Guide*\.