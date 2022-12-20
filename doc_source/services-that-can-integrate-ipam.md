# Amazon VPC IP Address Manager \(IPAM\) and AWS Organizations<a name="services-that-can-integrate-ipam"></a>

Amazon VPC IP Address Manager \(IPAM\) is a VPC feature that makes it easier for you to plan, track, and monitor IP addresses for your AWS workloads\.

Using AWS Organizations allows you to monitor IP address usage throughout your organization and share IP address pools across member accounts\.



For more information, see  [ Integrate IPAM with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/ipam/enable-integ-ipam.html) in the *Amazon VPC IPAM User Guide*\. 

Use the following information to help you integrate Amazon VPC IP Address Manager \(IPAM\) with AWS Organizations\. 

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-ipam"></a>

The following service\-linked role is automatically created in your organization's management account and each member account when you integrate IPAM with AWS Organizations either by using the IPAM console or using IPAM's `EnableIpamOrganizationAdminAccount` API\. 
+ `AWSServiceRoleForIPAM`

For more information, see [ Service\-linked roles for IPAM](https://docs.aws.amazon.com/vpc/latest/ipam/iam-ipam-slr.html) in the *Amazon VPC IPAM User Guide*\. 

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-ipam"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by IPAM grant access to the following service principals:
+ `ipam.amazonaws.com`

## To enable trusted access with IPAM<a name="integrate-enable-ta-ipam"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

**Note**  
When you designate a delegated administrator for IPAM it automatically enables trusted access for IPAM for your organization\.  
IPAM requires trusted access to AWS Organizations before you can designate a member account to be the delegated administrator for this service for your organization\.

You can enable trusted access using only Amazon VPC IP Address Manager \(IPAM\) tools\.

If you integrate IPAM with AWS Organizations using the IPAM console or using the IPAM `EnableIpamOrganizationAdminAccount` API, you automatically grant trusted access to IPAM\. Granting trusted access creates the service\-linked role ` AWSServiceRoleForIPAM` in the management account and in all of the member accounts in the organization\. IPAM uses the service\-linked role to monitor CIDRs associated with EC2 networking resources in your organization and to store metrics related to IPAM in Amazon CloudWatch\. For more information, see [Service\-linked roles for IPAM](https://docs.aws.amazon.com/vpc/latest/ipam/iam-ipam-slr.html) in the *Amazon VPC IPAM User Guide*\. 

 For instructions about enabling trusted access, see [Integrate IPAM with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/ipam/enable-integ-ipam.html) in the *Amazon VPC IPAM User Guide*\. 

**Note**  
 You can't enable trusted access with IPAM using the AWS Organizations console or with the [https://docs.aws.amazon.com/ cli/latest/reference/organizations/enable-aws-service-access.html](https://docs.aws.amazon.com/ cli/latest/reference/organizations/enable-aws-service-access.html) API\. 

## To disable trusted access with IPAM<a name="integrate-disable-ta-ipam"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with IPAM using the AWS Organizations `disable-aws-service-access` API\. 

 For information about disabling IPAM account permissions and deleting the service\-linked role, see [Service\-linked roles for IPAM](https://docs.aws.amazon.com/ /vpc/latest/ipam/iam-ipam-slr.html) in the *Amazon VPC IPAM User Guide*\. 

You can disable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable Amazon VPC IP Address Manager \(IPAM\) as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal ipam.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for IPAM<a name="integrate-enable-da-ipam"></a>

The delegated administrator account for IPAM is responsible for creating the IPAM and IP address pools, managing and monitoring IP address usage in the organization, and sharing IP address pools across member accounts\. For more information, see [Integrate IPAM with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/ipam/enable-integ-ipam.html) in the *Amazon VPC IPAM User Guide*\.

Only an administrator in the organization management account can configure a delegated administrator for IPAM\.

You can specify a delegated administrator account from the IPAM console, or by using the `enable-ipam-organization-admin-account` API\. For more information, see [enable\-ipam\-organization\-admin\-account](https://docs.aws.amazon.com/cli/latest/reference/ec2/enable-ipam-organization-admin-account.html) in the * AWS AWS CLI Command Reference*\. 

**Minimum permissions**  
Only an IAM user or role in the Organizations management account can configure a member account as a delegated administrator for IPAM in the organization

To configure a delegated administrator using the IPAM console, see [Integrate IPAM with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/ipam/enable-integ-ipam.html) in the *Amazon VPC IPAM User Guide*\.

## Disabling a delegated administrator for IPAM<a name="integrate-disable-da-ipam"></a>

Only an administrator in the organization management account can configure a delegated administrator for IPAM\.

 To remove a delegated administrator using the AWS AWS CLI, see [disable\-ipam\-organization\-admin\-account](https://docs.aws.amazon.com/cli/latest/reference/ec2/disable-ipam-organization-admin-account.html) in the *AWS AWS CLI Command Reference*\.

 To disable the delegated admin IPAM account using the IPAM console, see [Integrate IPAM with AWS Organizations](https://docs.aws.amazon.com/vpc/latest/ipam/enable-integ-ipam.html) in the *Amazon VPC IPAM User Guide*\.