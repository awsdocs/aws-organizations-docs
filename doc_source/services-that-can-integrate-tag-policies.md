# Tag policies and AWS Organizations<a name="services-that-can-integrate-tag-policies"></a>

*Tag policies* are a type of policy in AWS Organizations that can help you standardize tags across resources in your organization's accounts\. For more information about tag policies, see [Tag policies](orgs_manage_policies_tag-policies.md)\. 

Use the following information to help you integrate tag policies with AWS Organizations\.



## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-tag-policies"></a>

Organizations interacts with the tags attached to your resources using the following service principal\.
+ `tagpolicies.tag.amazonaws.com`

## Enabling trusted access for tag policies<a name="integrate-enable-ta-tag-policies"></a>

You can enable trusted access either by enabling tag policies in the organization, or by using the AWS Organizations console\.

**Important**  
We strongly recommend that you enable trusted access by enabling tag policies\. This enables Organizations to perform required setup tasks\.

You can enable trusted access for tag policies by enabling the tag policy type in the AWS Organizations console\. For more information, see [Enabling a policy type](orgs_manage_policies_enable-disable.md#enable-policy-type)\.

You can enable trusted access by using either the AWS Organizations console, by running a AWS CLI command, or by calling an API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **tag policies**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of tag policies that they can now enable that service using its console to work with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the OrganizationsCLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable tag policies as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \ 
      --service-principal tagpolicies.tag.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with tag policies<a name="integrate-disable-ta-tag-policies"></a>

You can disable trusted access for tag policies by disabling the tag policy type in the AWS Organizations console\. For more information, see [Disabling a policy type](orgs_manage_policies_enable-disable.md#disable-policy-type)\. 