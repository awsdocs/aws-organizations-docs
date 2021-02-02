# AWS CloudTrail and AWS Organizations<a name="services-that-can-integrate-cloudtrail"></a>

AWS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account\. Using AWS CloudTrail, a user in a management account can create an organization trail that logs all events for all AWS accounts in that organization\. Organization trails are automatically applied to all member accounts in the organization\. Member accounts can see the organization trail, but can't modify or delete it\. By default, member accounts don't have access to the log files for the organization trail in the Amazon S3 bucket\. This helps you uniformly apply and enforce your event logging strategy across the accounts in your organization\.

For more information, see [ Creating a Trail for an Organization](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/creating-trail-organization.html) in the *AWS CloudTrail User Guide*\. 

Use the following information to help you to help you integrate AWS CloudTrail with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-cloudtrail"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow CloudTrail to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between CloudTrail and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForCloudTrail`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-cloudtrail"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by CloudTrail grant access to the following service principals:
+ `cloudtrail.amazonaws.com`

## Enabling trusted access with CloudTrail<a name="integrate-enable-ta-cloudtrail"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS CloudTrail console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS CloudTrail console or tools to enable integration with Organizations\. This lets AWS CloudTrail perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS CloudTrail\.For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS CloudTrail console or tools then you don’t need to complete these steps\.

You must sign in with your AWS Organizations management account to create an organization trail\. If you create the trail from the AWS CloudTrail console, trusted access is configured automatically for you\. If you choose to create an organization trail using the AWS CLI or the AWS API, you must manually configure trusted access\. For more information, see [ Enabling CloudTrail as a trusted service in AWS Organizations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.html#cloudtrail-create-organization-trail-by-using-the-cli-enable-trusted-service) in the *AWS CloudTrail User Guide\.*

You can enable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [aws organizations enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS CloudTrail as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \
      --service-principal cloudtrail.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with CloudTrail<a name="integrate-disable-ta-cloudtrail"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

AWS CloudTrail requires trusted access with AWS Organizations to work with organization trails\. If you disable trusted access using AWS Organizations while you're using AWS CloudTrail for organization trails, the trails stop functioning for member accounts because CloudTrail can't access the organization\. The organization trails remain, as does the `AWSServiceRoleForCloudTrail` role created for integration between CloudTrail and AWS Organizations\. If you re\-enable trusted access, CloudTrail continues to operate as before, without the need for you to reconfigure the trails\.

Only an administrator in the AWS Organizations management account can disable trusted access with AWS CloudTrail\.

You can disable trusted access using only the Organizations console or tools\.

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ Old console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Settings](https://console.aws.amazon.com/organizations/home#/organization/settings)** tab under **Trusted access for AWS services**, find the row for **AWS CloudTrail** and then choose **Disable access**\.

1. In the dialog box, choose **Disable access for *service\-name***\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudTrail that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ New console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS CloudTrail** and then choose the service’s name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudTrail that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [aws organizations disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS CloudTrail as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal cloudtrail.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------