# AWS Single Sign\-On and AWS Organizations<a name="services-that-can-integrate-peregrine"></a>

AWS Single Sign\-On \(AWS SSO\) provides single sign\-on services for all of your AWS accounts and cloud applications\. It connects with Microsoft Active Directory through AWS Directory Service to allow users in that directory to sign in to a personalized user portal using their existing Active Directory user names and passwords\. From the portal, users have access to all the AWS accounts and cloud applications that you provide in the portal\. For more information about AWS SSO, see the [AWS Single Sign\-On User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/)\.

The following list provides information that is useful to know when you want to integrate AWS SSO and AWS Organizations:
+ **To enable trusted access with AWS Organizations:** AWS SSO requires trusted access with AWS Organizations to function\. Trusted access is enabled when you set up AWS SSO\. For more information, see [Getting Started \- Step 1: Enable AWS Single Sign\-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/step1.html) in the *AWS Single Sign\-On User Guide*\.
+ **To disable trusted access with AWS Organizations:** AWS SSO requires trusted access with AWS Organizations to operate\. If you disable trusted access using AWS Organizations while you are using AWS SSO, it stops functioning because it can't access the organization\. Users can't use AWS SSO to access accounts\. Any roles that AWS SSO creates remain, but the AWS SSO service can't access them\. The AWS SSO service\-linked roles remain\. If you reenable trusted access, AWS SSO continues to operate as before, without the need for you to reconfigure the service\. 

  If you remove an account from your organization, AWS SSO automatically cleans up any metadata and resources, such as its service\-linked role\. A standalone account that is removed from an organization no longer works with AWS SSO\.
+ **Service principal name for AWS SSO: ** `sso.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts when trusted access is enabled: ** `AWSServiceRoleForSSO`

  For more information, see [Using Service\-Linked Roles for AWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/using-service-linked-roles.html) in the *AWS Single Sign\-On User Guide*\.