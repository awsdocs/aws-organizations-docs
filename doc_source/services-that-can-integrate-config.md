# AWS Config and AWS Organizations<a name="services-that-can-integrate-config"></a>

Multi\-account, multi\-region data aggregation in AWS Config enables you to aggregate AWS Config data from multiple accounts and AWS Regions into a single account\. Multi\-account, multi\-region data aggregation is useful for central IT administrators to monitor compliance for multiple AWS accounts in the enterprise\. An aggregator is a resource type in AWS Config that collects AWS Config data from multiple source accounts and Regions\. Create an aggregator in the Region where you want to see the aggregated AWS Config data\. While creating an aggregator, you can choose to add either individual account IDs or your organization\. For more information about AWS Config, see the [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/)\.

You can also use [AWS Config APIs](https://docs.aws.amazon.com/config/latest/APIReference/welcome.html) to manage AWS Config rules across all AWS accounts in your organization\. For more information, see [Enabling AWS Config Rules Across All Accounts in Your Organization](https://docs.aws.amazon.com/config/latest/developerguide/config-rule-multi-account-deployment.html) in the *AWS Config Developer Guide*\.

The following list provides information that is useful to know when you want to integrate AWS Config and AWS Organizations:
+ **To enable trusted access with AWS Organizations:** To enable trusted access to AWS Organizations from AWS Config, you create a multi\-account aggregator and add the organization\. For information on how to configure a multi\-account aggregator, see [Setting Up an Aggregator Using the Console](https://docs.aws.amazon.com/config/latest/developerguide/setup-aggregator-console.html) in the *AWS Config Developer Guide*\.
+ **Service principal name**
  + For AWS Config: `config.amazonaws.com`
  + For AWS Config rules: `config-multiaccountsetup.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts** when trusted access is enabled
  + For AWS Config: `AWSConfigRoleForOrganizations`
  + For AWS Config rules: `AWSServiceRoleForConfigMultiAccountSetup` 