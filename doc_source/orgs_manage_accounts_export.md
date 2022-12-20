# Exporting all AWS account details for your organization<a name="orgs_manage_accounts_export"></a>

With AWS Organizations, management account users and delegated administrators for an organization can export a \.csv file with all account details within an organization\. As a result, organization administrators can easily view accounts and filter by status: `ACTIVE`, `SUSPENDED`, or `PENDING`\. If your organization has many accounts, the \.csv file download option provides an easy way to view and sort account details in a spreadsheet\.

Previously, the only way to view accounts was to look at the account hierarchy or list display in the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. 

**Note**  
Only principals in the management account can download the account list\.

## Exporting a list of all AWS accounts in your organization<a name="orgs_manage_accounts_export_csv"></a>

When you sign in to the organization's management account, you can get a list of all accounts that are part of your organization as a \.csv file\. The list contains individual account details; however, it doesn't specify to which organizational unit \(OU\) the account belongs\.

The \.csv file contains the following information for each account:
+ **Account ID** \- Numeric account identifier\. For example: 123456789012 
+ **ARN** \- Amazon Resource Name for the account\. For example: `arn:aws:organizations::123456789012account/o-o1gb0d1234/123456789012` 
+ **Email** \- Email address associated with the account\. For example: marymajor@example\.com 
+ **Name** \- Account name provided by account creator\. For example: stage testing account 
+ **Status** \- Account status within the organization\. Value can be `PENDING`, `ACTIVE` or `SUSPENDED`\.
+ **Joined method** \- Specifies how the account was created\. Value can be `INVITED`, `CREATED` or `ADDED`\.
+ **Joined timestamp** \- Date and time the account joined the organization\.

**Minimum permissions**  
To export a \.csv file with all member accounts in your organization, you must have the following permissions:  
`organizations:DescribeOrganization` 
`organizations:ListAccounts` 

------
#### [ AWS Management Console ]

**To export a \.csv file for all AWS accounts in your organization**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. Choose **Actions**, then for **AWS account** choose **Export account list**\. The blue banner at the top of the page indicates "Export is in progress\!"

1.  When the file is ready, the banner turns green and indicates: "Download is ready\!" Choose **Download CSV**\. The file `Organization_accounts_information.csv` downloads to your device\.

------
#### [ AWS CLI & AWS SDKs ]

The only way to export the \.csv file with account details is by using the AWS Management Console\. You can't export the account list \.csv file using the AWS CLI\. 

------