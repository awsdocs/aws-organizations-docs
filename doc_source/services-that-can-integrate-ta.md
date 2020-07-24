# AWS Trusted Advisor and AWS Organizations<a name="services-that-can-integrate-ta"></a>

AWS Trusted Advisor inspects your AWS environment and makes recommendations when opportunities exist to save money, to improve system availability and performance, or to help close security gaps\. When integrated with Organizations, you can receive Trusted Advisor check results for all of the accounts in your organization and download reports to view the summaries of your checks and any affected resources\.

For more information, see [Organizational view for AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html) in the *AWS Support User Guide*\.

The following list provides information that is useful to know when you want to integrate Trusted Advisor and Organizations:
+ **To enable trusted access with AWS Organizations:** You must sign in as an IAM user or role in your AWS Organizations master account\. For information, see [Enable organizational view](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html#enable-organizational-view) in the *AWS Support User Guide*\.
+ **To disable trusted access with AWS Organizations:** You must sign in as an IAM user or role in your AWS Organizations master account\. After you disable this feature, Trusted Advisor stops recording check information for all other accounts in your organization\. You can't view or download existing reports or create new reports\. For information, see [Disable organizational view](https://docs.aws.amazon.com/awssupport/latest/user/organizational-view.html#disable-organizational-view) in the *AWS Support User Guide*\.
+ **Service principal name for Trusted Advisor: ** `reporting.trustedadvisor.amazonaws.com`
+ **Role name created to synchronize with Trusted Advisor: ** `AWSServiceRoleForTrustedAdvisorReporting`