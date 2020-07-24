# AWS Systems Manager and AWS Organizations<a name="services-that-can-integrate-systems-manager"></a>

AWS Systems Manager is a collection of capabilities that enable visibility and control of your AWS resources\. One of these capabilities, Systems Manager Explorer, is a customizable operations dashboard that reports information about your AWS resources\. You can synchronize operations data across all AWS accounts in your organization by using Organizations and Systems Manager Explorer\. For more information, see [Systems Manager Explorer](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer.html)\.

To integrate Systems Manager and Organizations, [all features](orgs_manage_org_support-all-features.md) must be enabled for your organization\.

The following list provides information that is useful to know when you want to integrate Systems Manager and Organizations:
+ **To enable trusted access with AWS Organizations** – You must sign in with your AWS Organizations master account and create a Resource Data Sync\. For information, see [Setting Up Explorer to Display Data from Multiple Accounts and Regions](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer-resource-data-sync.html) in the *AWS Systems Manager User Guide*\.
+ **To disable trusted access with AWS Organizations** – Systems Manager requires trusted access with AWS Organizations to synchronize operations data across AWS accounts in your organization\. If you disable trusted access, then Systems Manager fails to synchronize operations data and reports an error\. To reenable trusted access, you must create a new Resource Data Sync for Systems Manager Explorer\. 
+ **Service principal name for Systems Manager: ** `ssm.amazonaws.com`
+ **Role name created to synchronize with Systems Manager Explorer: ** `AWSServiceRoleForAmazonSSM_AccountDiscovery`