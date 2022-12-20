# Amazon Macie and AWS Organizations<a name="services-that-can-integrate-macie"></a>

Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover, monitor, and help you protect your sensitive data in Amazon Simple Storage Service \(Amazon S3\)\. Macie automates the discovery of sensitive data, such as personally identifiable information \(PII\) and intellectual property, to provide you with a better understanding of the data that your organization stores in Amazon S3\.

For more information, see [Managing Amazon Macie accounts with AWS Organizations](https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html) in the *[Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/userguide/)*\.

Use the following information to help you integrate Amazon Macie with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-macie"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created for your organization's delegated Macie administrator account when you enable trusted access\. This role allows Macie to perform supported operations for the accounts in your organization\.

You can delete this role only if you disable trusted access between Macie and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleRorAmazonMacie`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-macie"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Macie grant access to the following service principals:
+ `macie.amazonaws.com`

## Enabling trusted access with Macie<a name="integrate-enable-ta-macie"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the Amazon Macie console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the Amazon Macie console or tools to enable integration with Organizations\. This lets Amazon Macie perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by Amazon Macie\. For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the Amazon Macie console or tools then you don’t need to complete these steps\.

**To enable trusted access using the Macie console**  
Amazon Macie requires trusted access to AWS Organizations to designate a member account to be the Macie administrator for your organization\. If you configure a delegated administrator using the Macie management console, then Macie automatically enables trusted access for you\.

For more information, see [Integrating and configuring an organization in Amazon Macie](https://docs.aws.amazon.com/macie/latest/user/accounts-mgmt-ao-integrate.html) in the *Amazon Macie User Guide*\.

You can enable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable Amazon Macie as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \
      --service-principal macie.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for Macie<a name="integrate-enable-da-macie"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for Macie that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Macie\.

**Minimum permissions**  
Only an IAM user or role in the Organizations management account with the following permissions can configure a member account as a delegated administrator for Macie in the organization:  
`organizations:EnableAWSServiceAccess`
`macie:EnableOrganizationAdminAccount`

**To designate a member account as a delegated administrator for Macie**  
Amazon Macie requires trusted access to AWS Organizations to designate a member account to be the Macie administrator for your organization\. If you configure a delegated administrator using the Macie management console, then Macie automatically enables trusted access for you\.

For more information, see [https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin](https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin)