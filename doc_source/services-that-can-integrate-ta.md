# AWS Trusted Advisor and AWS Organizations<a name="services-that-can-integrate-ta"></a>

AWS Trusted Advisor inspects your AWS environment and makes recommendations when opportunities exist to save money, to improve system availability and performance, or to help close security gaps\. When integrated with Organizations, you can receive Trusted Advisor check results for all of the accounts in your organization and download reports to view the summaries of your checks and any affected resources\.

For more information, see [Organizational view for AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html) in the *AWS Support User Guide*\.

Use the following information to help you integrate AWS Trusted Advisor with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-ta"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's accounts when you enable trusted access\. These roles allow Trusted Advisor to perform supported operations within the accounts in your organization\.

You can delete or modify these roles only if you disable trusted access between Trusted Advisor and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForTrustedAdvisorReporting`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-ta"></a>

The service\-linked roles in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Trusted Advisor grant access to the following service principals:
+ `reporting.trustedadvisor.amazonaws.com`

## Enabling trusted access with Trusted Advisor<a name="integrate-enable-ta-ta"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using only the AWS Trusted Advisor tools\.

**To enable trusted access using the Trusted Advisor console**  
See [Enable organizational view](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html#enable-organizational-view) in the *AWS Support User Guide*\.

## Disabling trusted access with Trusted Advisor<a name="integrate-disable-ta-ta"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

After you disable this feature, Trusted Advisor stops recording check information for all other accounts in your organization\. You can't view or download existing reports or create new reports\. 

You can disable trusted access using either the AWS Trusted Advisor or AWS Organizations tools\.

**Important**  
We strongly recommend that whenever possible, you use the AWS Trusted Advisor console or tools to disable integration with Organizations\. This lets AWS Trusted Advisor perform any clean up that it requires, such as deleting resources or access roles that are no longer needed by the service\. Proceed with these steps only if you can’t disable integration using the tools provided by AWS Trusted Advisor\.  
If you disable trusted access by using the AWS Trusted Advisor console or tools then you don’t need to complete these steps\.

**To disable trusted access using the Trusted Advisor console**  
 See [Disable organizational view](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html#disable-organizational-view) in the *AWS Support User Guide*\.

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS Trusted Advisor as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal reporting.trustedadvisor.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------