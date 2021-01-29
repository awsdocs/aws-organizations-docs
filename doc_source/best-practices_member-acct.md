# Best practices for member accounts<a name="best-practices_member-acct"></a>

Follow these recommendations to help protect the security of the member accounts in in your organization\. These recommendations assume that you also adhere to the[ best practice of using the root user only for those tasks that truly require it](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html)\.

**Topics**
+ [Use a group email address for all member account root users](#best-practices_member-acct_email-address)
+ [Use a complex password for member account root user](#best-practices_mbr-acct_complex-password)
+ [Enable MFA for your root user credentials](#best-practices_mbr-acct_mfa)
+ [Add the management account's phone number to the member account contact information](#best-practices_mbr-acct_phone-number)
+ [Review and keep track of who has access](#best-practices_mbr-acct_review-access)
+ [Document the processes for using the root user credentials](#best-practices_mbr-acct_document-processes)
+ [Use an SCP to restrict what the root user in your member accounts can do](#best-practices_mbr-acct_scp)
+ [Apply controls to monitor access to the root user credentials](#best-practices_mbr-acct_monitor-access)

## Use a group email address for all member account root users<a name="best-practices_member-acct_email-address"></a>
+ Use an email address that is managed by your business\. Do not use a public email provider or one that is managed by a third party\.
+ Use an email address that forwards received messages directly to a list of senior business managers\. In the event that AWS needs to contact the owner of the account, for example, to confirm access, the email is distributed to multiple parties\. This approach helps to reduce the risk of delays in responding, even if individuals are on vacation, out sick, or leave the business\.

## Use a complex password for member account root user<a name="best-practices_mbr-acct_complex-password"></a>
+ The security of your account's root user depends on the strength of its password\. We recommend that you use a password that is long, complex, and not used anywhere else\. Numerous password managers and complex password generation algorithms and tools can help you achieve these goals\.
+ If you are using a strong password, as described in the previous point, and you rarely access the root user, we recommend that you do ***not*** periodically change the password\. Changing the password more frequently than you use it increases the risk of compromise\.
+ Rely on your business' information security policy for managing long\-term storage and access to the passwords for your member account root users\. Unlike the management account, however, it's reasonable to consider storing the password in a credible and business\-approved password manager system or tool\.

  Store the password in a password manager system or tool under further controls and processes\. If you do use a password manager, we recommend that it be offline\. To avoid creating a circular dependency, do not store the password with tools that depend on AWS services that you sign in to with the protected account\. 

  Whatever method you choose, we recommend that the method is resilient and requires multiple actors to be involved to reduce collusion risks\.
+ Alternatively, you can store the password for a member account root user in a safe, [using the guidance we provide for the management account root user](orgs_best-practices_mgmt-acct.md#best-practices_mgmt-acct_complex-password)\.
+ Consider not enabling credentials for the root user in created member accounts\. By default, Organizations assigns a random, complex, and very long password that you can't use\. To access the root user, you must perform the steps for password recovery\. We recommend that you don't do this unless you need to perform a task that can only be performed by the root user in the account\. For more information, see [Accessing a member account as the root user](orgs_manage_accounts_access.md#orgs_manage_accounts_access-as-root)\.
+ However you choose to handle the root user's password, you can apply a service control policy \(SCP\) that prevents the member account root users from calling any AWS APIs\. However, if you need to respond to a significant security event in a member account, you might need rapid access to that account's root user\. Therefore, using a complex password for the member account root user and creating procedures for access and use in advance is still the recommended approach, as described in the previous points\.

## Enable MFA for your root user credentials<a name="best-practices_mbr-acct_mfa"></a>

For instructions on how to enable multi\-factor authentication \(MFA\), see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html)\.
+ We recommend that you use a hardware\-based device that does not rely on a battery to generate the one\-time password \(OTP\)\. This approach helps ensure that the MFA is impossible to duplicate and isn't subject to battery fade risks while in long\-term storage 
  + If you do decide to use a battery\-based MFA, be sure to add processes to check the device periodically, and replace it when the expiry date approaches\.
  + Create a plan to handle the logistics of needing to maintain 24/7 access to the token in case it is needed\.
+ If you choose to use a virtual MFA application, then unlike our [recommendation for the management account root user](orgs_best-practices_mgmt-acct.md#best-practices_mgmt-acct_mfa), for member accounts you can re\-use a single MFA device for multiple member accounts\. You can address geographic limitations by printing and securely storing the QR code used to configure the account in the virtual MFA application\. Document the QR code's purpose, and seal and store it in accessible safes across the time zones you operate in, according to your information security policy\. Then, when access is needed in a different geographic location, the local copy of the QR code can be retrieved and used to configure a virtual MFA app in the new location\. 
+ Store the MFA device according to your information security policy but ***not*** in the same place as the associated password for the root user\. Make sure that the process to access the password and the process to access the MFA device require different access procedures by different resources \(people, data and tools\)\. 
+ Any access of the MFA device or its storage location should be logged and monitored\. 
+ If you lose or break your MFA device, you might need to contact Customer Support to remove the MFA from your account\. Before they can do that, they must verify that the person making the request is in possession of the email address, phone number, and security questions associated with the account\. So ensure you have that information and keep it up to date and securely stored\.

## Add the management account's phone number to the member account contact information<a name="best-practices_mbr-acct_phone-number"></a>
+ You can normally rely on the [phone number from the organization's management account](orgs_best-practices_mgmt-acct.md#best-practices_mgmt-acct_phone-number) for any critical account recovery\. Therefore, we believe it’s unnecessary operational overhead to manage a separate telephone number to the contact information for a member account\. Therefore, we recommend that you add the same phone number as the management account\. Whether you use the same number as the management account or not, keep an accurate list of the phone numbers used, and any active security questions in a secure location similarly to the credentials themselves\.

## Review and keep track of who has access<a name="best-practices_mbr-acct_review-access"></a>
+ [As we recommended for the management account](orgs_best-practices_mgmt-acct.md#best-practices_mgmt-acct_review-access), you should periodically review the personnel within your business who have access to the email address, password, MFA, and phone number for your member account's root user\. Align your review with existing business procedures\. However, it’s worth adding a monthly or quarterly review of this information to ensure that only the correct people have access\.
+ Ensure that the process to recover or reset access to the root user credentials is not reliant on any specific individual to complete\. All processes should address the possibility of people being unavailable\.
+ Ensure that the process to recover or reset access to the root user credentials can't be completed by a single individual\. It’s important that this process require the cooperation of multiple personnel to complete\.

## Document the processes for using the root user credentials<a name="best-practices_mbr-acct_document-processes"></a>
+ It’s common for important processes, such as the creation of the organization's management account, to be a planned process including multiple steps with multiple personnel\. We recommend that you document and publish that plan, including the steps to be performed and their sequence of completion\. This approach helps ensure that the decisions made are followed correctly\.
+ Document the performance of important processes as they are performed to ensure you have a record of the individuals involved in each step and the values used\. It’s also important to provide documentation about any exceptions and unforeseen events that occur\.

  If an exception or unforeseen event does occur, document the time it occurred, who left the room, and what was taken out\. You should then also document who returned to the room and what was brought back in\. 
+ Create and publish processes about how to use the root user credentials in different scenarios, such as resetting the password\. If you are unsure about the process for interacting with AWS Support in a specific scenario, create a support ticket to ask for the latest guidance about how to perform that task\.

  Some of the scenarios you should document include the following:
  + Accessing the root user to perform one of the operations that only the root user can perform\. 
  + Resetting a root user password when you lose access\.
  + Changing the root user password when you still have access\.
  + Resetting the root user MFA when you lose access to the device\.
  + Changing the root user MFA when you use a battery\-based device\.
  + Resetting the root user email address when you lose access to the email account\.
  + Changing the root user email address when you still have access\.
  + Resetting the root user phone number when you lose access to the phone number\.
  + Changing the root user phone number when you still have access\.
  + Deleting the organization's management account\.
+ Test and validate that you continue to have access to the root user and that the mobile phone number for the member account \(if you assigned one\) is operational on at least a quarterly basis\. This schedules helps assure the business that the process works and that you maintain access\. It also demonstrates that those custodians of the access understand the steps they need to perform for the process to be successful\. You never want to be in a position where the personnel involved in a process don’t understand what they are supposed to do\. As with fire drills, practice develops competency and reduces surprises\.

  With each test, take the opportunity to reflect on the experience and propose improvements to the process\. Especially examine any steps that were performed incorrectly or resulted in unexpected results\. How could you change the process to improve it the next time?

  Some customers use these tests as an opportunity to rotate passwords\. Our recommendation is to not do this, and to maintain the same complex password\. You should only look to updating the password if you suspect it was compromised\.

## Use an SCP to restrict what the root user in your member accounts can do<a name="best-practices_mbr-acct_scp"></a>

We recommend that you create a service control policy \(SCP\) in the organization and attach it to the organization's root so that it applies to all member accounts\. The following example SCP prevents the root user in any member account from being able to make any AWS service API calls\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "*",
            "Condition": {
                "StringLike": { "aws:PrincipalArn": "arn:aws:iam::*:root" }
            }
        }
    ]
}
```

In the majority of circumstances, any administrative tasks can be performed by either an AWS Identity and Access Management \(IAM\) role in the member account that has relevant administrator permissions\. Any such roles should have suitable controls applied that limit, log, and monitor activity\. 

## Apply controls to monitor access to the root user credentials<a name="best-practices_mbr-acct_monitor-access"></a>
+ If you do choose to enable access to the root user credentials, such access should be a rare event\. Create alerts using tools like Amazon CloudWatch Events to announce the login and use of the root user credentials\. This announcement should be significant and hard to miss, whether the use is valid or malicious\. For an example, see [Monitor and notify on AWS account root user activity](https://aws.amazon.com/blogs/mt/monitor-and-notify-on-aws-account-root-user-activity/)\.
+ Ensure that personnel who receive such an announcement understand how to validate that the root user access is expected, and how to escalate if they believe that a security incident is in progress\.