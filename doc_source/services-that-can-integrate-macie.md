# Amazon Macie and AWS Organizations<a name="services-that-can-integrate-macie"></a>

Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover, monitor, and help you protect your sensitive data in Amazon Simple Storage Service \(Amazon S3\)\. Macie automates the discovery of sensitive data, such as personally identifiable information \(PII\) and intellectual property, to provide you with a better understanding of the data that your organization stores in Amazon S3\.

For more information, see [Managing multiple Amazon Macie accounts with AWS Organizations](https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html) in the *[Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/userguide/)*\.

Use the following information to help you to help you integrate Amazon Macie with AWS Organizations\.

**Topics**
+ [Service\-linked roles created when you enable integration](#integrate-enable-slr-macie)
+ [Service principals used by the servie\-linked roles](#integrate-enable-svcprin-macie)
+ [Enabling trusted access with Macie](#integrate-enable-ta-macie)
+ [Enabling a delegated administrator account for Macie](#integrate-enable-da-macie)

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-macie"></a>

The following [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) are automatically created in your organization's accounts when you enable trusted access\. These roles allow Macie to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Macie and Organizations or if the account is removed from the organization\.
+ `AWSServiceRoleRorAmazonMacie`

## Service principals used by the servie\-linked roles<a name="integrate-enable-svcprin-macie"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Macie grant access to the following service principals:
+ `macie.amazonaws.com`

## Enabling trusted access with Macie<a name="integrate-enable-ta-macie"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

**To enable trusted access using the Macie console**  
Amazon Macie requires trusted access to AWS Organizations to designate a member account to be the Macie administrator for your organization\. If you configure a delegated administrator using the Macie management console, then Macie automatically enables trusted access for you\.

For more information, see [https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin](https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin)

**To enable trusted access using the AWS CLI or AWS SDK**  
If you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, then you must explicitly call the [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html) operation and provide the service principal as a parameter\. Then you can call [EnableOrganizationAdminAccount](https://docs.aws.amazon.com/macie/latest/APIReference/admin.html#EnableOrganizationAdminAccount) to delegate the Macie administrator account\.

As an example:

```
$ aws organizations enable-aws-service-access \
    --service-principal macie.amazonaws.com>
```

This command produces no output when successful\.

Follow that command with this one to complete the process:

```
$ aws macie2 enable-organization-admin-account \
    --admin-account-id 123456789012
```

This command produces no output when successful\.

For more information, see [Enable Trusted Access with AWS Organizations](https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin) in the *Amazon Macie User Guide*\.

## Enabling a delegated administrator account for Macie<a name="integrate-enable-da-macie"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for Macie that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of Macie\.

**Minimum permissions**  
Only an IAM user or role in the Organizations management account with the following permissions can configure a member account as a delegated administrator for Macie in the organization:  
`organizations:EnableAWSServiceAccess`
`macie:EnableOrganizationAdminAccount`

**To designate a member account as a delegated administrator for Macie**  
Amazon Macie requires trusted access to AWS Organizations to designate a member account to be the Macie administrator for your organization\. If you configure a delegated administrator using the Macie management console, then Macie automatically enables trusted access for you\.

For more information, see [https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin](https://docs.aws.amazon.com/macie/latest/user/macie-organizations.html#register-delegated-admin)