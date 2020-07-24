# AWS RAM and AWS Organizations<a name="services-that-can-integrate-ram"></a>

AWS Resource Access Manager \(AWS RAM\) enables you to share specified AWS resources that you own with other AWS accounts\. It's a centralized service that provides a consistent experience for sharing different types of AWS resources across multiple accounts\. For more information about AWS RAM, see the [https://docs.aws.amazon.com/ram/latest/userguide/what-is.html](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html)\.

The following list provides information that you need when integrating AWS RAM with AWS Organizations:
+ **To enable trusted access with AWS Organizations:** From the AWS RAM CLI, use the `enable-sharing-with-aws-organizations` command\. For more information, see [Sharing Your Resources](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html) in the *AWS RAM User Guide*\.
+ **Service principal name for AWS RAM: ** `ram.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts when trusted access is enabled: ** `AWSResourceAccessManagerServiceRolePolicy`