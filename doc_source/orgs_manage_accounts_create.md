# Creating an AWS account in your organization<a name="orgs_manage_accounts_create"></a>

This page describes how to create accounts within your organization in AWS Organizations\. To learn about getting started with AWS and creating a single AWS account, see the [Getting Started Resource Center](http://aws.amazon.com/getting-started/)\.

An organization is a collection of AWS accounts that you centrally manage\. You can perform the following procedures to manage the accounts that are part of your organization:
+ [Creating an AWS account that is part of your organization](#orgs_manage_accounts_create-new)
+ [Accessing a member account that has a management account access role](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)

**Important**  
When you create a member account in your organization, AWS Organizations automatically creates an AWS Identity and Access Management \(IAM\) role in the member account\. This role enables IAM users in the management account who assume the role to exercise full administrative control over the member account\. This role is subject to any [service control policies \(SCPs\)](orgs_manage_policies_scps.md) that apply to the member account\.  
AWS Organizations also automatically creates a service\-linked role named `AWSServiceRoleForOrganizations` that enables integration with select AWS services\. You must configure the other services to allow the integration\. For more information, see [AWS Organizations and service\-linked roles](orgs_integrate_services.md#orgs_integrate_services-using_slrs)\.
If this organization is managed with AWS Control Tower, then create your accounts by using the AWS Control Tower account factory in the AWS Control Tower console or APIs\. If you create an account in Organizations, then that account isn't enrolled with AWS Control Tower\. For more information, see [Referring to Resources Outside of AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/external-resources.html#ungoverned-resources) in the *AWS Control Tower User Guide*\.

**Note**  
AWS accounts that you create as part of an organization are not automatically subscribed to AWS marketing emails\. To opt\-in your accounts to receive marketing emails, see [https://pages.awscloud.com/communication-preferences](https://pages.awscloud.com/communication-preferences)\.

## Creating an AWS account that is part of your organization<a name="orgs_manage_accounts_create-new"></a>

When you sign in to the organization's management account, you can create member accounts that are automatically part of your organization\. To do this, complete the following steps\.

When you create an account using the following procedure, Organizations automatically copies the following information from the management account to the new member account:
+ Account name
+ Phone number
+ Company name
+ Customer URL
+ Company contact email
+ Communication language 
+ Marketplace \(vendor of the account in some AWS Regions\)

AWS does ***not*** automatically collect all the information required for an account to operate as a standalone account\. If you ever need to remove the account from the organization and make it a standalone account, you must provide that information for the account before you can remove it\. For more information, see [Leaving an organization as a member account](orgs_manage_accounts_remove.md#orgs_manage_accounts_leave-as-member)\.

**Minimum permissions**  
To create a member account in your organization, you must have the following permissions:  
`organizations:CreateAccount`
`organizations:DescribeOrganization` – required only when using the Organizations console
`iam:CreateServiceLinkedRole` \(granted to principal `organizations.amazonaws.com` to enable creating the required service\-linked role in the member accounts\)\.

------
#### [ AWS Management Console ]

**To create an AWS account that is automatically part of your organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, choose **Add an AWS account**\.

1. On the **[Add an AWS account](https://console.aws.amazon.com/organizations/v2/home/accounts/add/create)** page, choose **Create an AWS account** \(it is chosen by default\)\. 

1. On the **[Create an AWS account](https://console.aws.amazon.com/organizations/v2/home/accounts/add/create)** page, for **AWS account name** enter the name that you want to assign to the account\. This name helps you distinguish the account from all other accounts in the organization and is separate from the IAM alias or the email name of the owner\.

1. For **Email address of the account's owner**, enter the email address of the account's owner\. This email address cannot already be associated with another AWS account because it becomes the user name credential for the root user of the account\.

1. \(Optional\) Specify the name to assign to the IAM role that is automatically created in the new account\. This role grants the organization's management account permission to access the newly created member account\. If you don't specify a name, AWS Organizations gives the role a default name of `OrganizationAccountAccessRole`\. We recommend that you use the default name across all of your accounts for consistency\.
**Important**  
Remember this role name\. You need it later to grant access to the new account for IAM users in the management account\.

1. \(Optional\) In the **Add tags** section, add one or more tags to the new account by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to an account\.

1. Choose **Create AWS account**\.
   + If you get an error that indicates that you exceeded your account quota for the organization, see [I get a "quota exceeded" message when I try to add an account to my organization](orgs_troubleshoot_general.md#troubleshoot_general_error-adding-account)\.
   + If you get an error that indicates that you can't add an account because your organization is still initializing, wait one hour and try again\.
   + You can also check the AWS CloudTrail log for information on whether the account creation was successful\. For more information, see [Logging and monitoring in AWS Organizations](orgs_security_incident-response.md)\.
   + If the error persists, contact [AWS Support](https://console.aws.amazon.com/support/home#/)\.

   The **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page appears, with your new account added to the list\.

1. Now that the account exists and has an IAM role that grants administrator access to users in the management account, you can access the account by following the steps in [Accessing and administering the member accounts in your organization](orgs_manage_accounts_access.md)\.

**Note**  
When you create an account, AWS Organizations initially assigns a long \(64 characters\), complex, randomly generated password to the root user\. You can't retrieve this initial password\. To access the account as the root user for the first time, you must go through the process for password recovery\. For more information, see [Accessing a member account as the root user](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.

------
#### [ AWS CLI & AWS SDKs ]

**To create an AWS account that automatically is part of your organization**  
You can use one of the following commands to create an account:
+ AWS CLI: [create\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-account.html)

  ```
  $ aws organizations create-account \
      --email susan@example.com \
      --account-name "Production Account"
  {
          "CreateAccountStatus": {
                  "State": "IN_PROGRESS",
                  "Id": "car-examplecreateaccountrequestid111"
          }
  }
  ```

  You can then check the status of the account creation with the following command\.

  ```
  $ aws organizations describe-create-account-status \
      --create-account-request-id car-examplecreateaccountrequestid111
  {
    "CreateAccountStatus": {
      "State": "SUCCEEDED",
      "AccountId": "555555555555",
      "AccountName": "Production account",
      "RequestedTimestamp": 1470684478.687,
      "CompletedTimestamp": 1470684532.472,
      "Id": "car-examplecreateaccountrequestid111"
    }
  }
  ```
+ AWS SDKs: [CreateAccount](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateAccount.html)

------