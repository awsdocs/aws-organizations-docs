# Amazon Detective and AWS Organizations<a name="services-that-can-integrate-detective"></a>

Amazon Detective uses your log data to generate visualizations that allow you to analyze, investigate, and identify the root cause of security findings or suspicious activity\. 

Using AWS Organizations allows you to ensure that your Detective behavior graph provides visibility into the activity for all of your organization accounts\.

When you grant trusted access to Detective, the Detective service can react automatically to changes in the organization membership\. The delegated administrator can enable any organization account as a member account in the behavior graph\. Detective also can automatically enable new organization accounts as member accounts\. Organization accounts cannot disassociate themselves from the behavior graph\.



For more information, see [Using Amazon Detective in your organization](https://docs.aws.amazon.com/detective/latest/adminguide/accounts-orgs-transition.html) in the *Amazon Detective Administration Guide*\. 

Use the following information to help you integrate Amazon Detective with AWS Organizations\. 

## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-detective"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows Detective to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between Detective and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForDetective`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-detective"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by Detective grant access to the following service principals:
+ `detective.amazonaws.com`

## To enable trusted access with Detective<a name="integrate-enable-ta-detective"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

**Note**  
When you designate a delegated administrator for Amazon Detective, Detective automatically enables trusted access for Detective for your organization\.  
Detective requires trusted access to AWS Organizations before you can designate a member account to be the delegated administrator for this service for your organization\.

You can enable trusted access using only the Organizations tools\.

You can enable trusted access by using the AWS Organizations console\.

------
#### [ AWS Management Console ]

**To enable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **Amazon Detective**, choose the service’s name, and then choose **Enable trusted access**\.

1. In the confirmation dialog box, enable **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Amazon Detective that they can now enable that service using its console to work with AWS Organizations\.

------

## To disable trusted access with Detective<a name="integrate-disable-ta-detective"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in the AWS Organizations management account can disable trusted access with Amazon Detective\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by using the AWS Organizations console\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **Amazon Detective** and then choose the service’s name\.

1. Choose **Disable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of Amazon Detective that they can now disable that service using its console or tools from working with AWS Organizations\.

------

## Enabling a delegated administrator account for Detective<a name="integrate-enable-da-detective"></a>

The delegated administrator account for Detective is the administrator account for a Detective behavior graph\. The delegated administrator determines which organization accounts to enable and disable as member accounts in that behavior graph\. The delegated administrator can configure Detective to automatically enable new organization accounts as member accounts as they are added to the organization\. For information on how a delegated administrator manages organization accounts, see [Managing organization accounts as member accounts](https://docs.aws.amazon.com/detective/latest/adminguide/accounts-orgs-members.html) in the *Amazon Detective Administration Guide*\.

Only an administrator in the organization management account can configure a delegated administrator for Detective\.

You can specify a delegated administrator account from the Detective console or API, or by using the Organizations CLI or SDK operation\. 

**Minimum permissions**  
Only an IAM user or role in the Organizations management account can configure a member account as a delegated administrator for Detective in the organization

To configure a delegated administrator using the Detective console or API, see [Designating a Detective administrator account for an organization](https://docs.aws.amazon.com/detective/latest/adminguide/accounts-designate-admin.html) in the *Amazon Detective Administration Guide*\.

------
#### [ AWS CLI, AWS API ]

If you want to configure a delegated administrator account using the AWS CLI or one of the AWS SDKs, you can use the following commands:
+ AWS CLI: 

  ```
  $ aws organizations register-delegated-administrator \
      --account-id 123456789012 \
      --service-principal detective.amazonaws.com
  ```
+ AWS SDK: Call the Organizations `RegisterDelegatedAdministrator` operation and the member account's ID number and identify the account service `principal account.amazonaws.com` as parameters\. 

------

## Disabling a delegated administrator for Detective<a name="integrate-disable-da-detective"></a>

You can remove the delegated administrator using either the Detective console or API, or by using the Organizations `DeregisterDelegatedAdministrator` CLI or SDK operation\. For information on how to remove a delegated administrator using the Detective console or API, or the Organizations API, see [Designating a Detective administrator account for an organization](https://docs.aws.amazon.com/detective/latest/adminguide/accounts-designate-admin.html) in the *Amazon Detective Administration Guide*\.