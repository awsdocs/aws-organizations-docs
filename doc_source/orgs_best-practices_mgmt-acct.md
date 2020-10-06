# Best practices for the master account<a name="orgs_best-practices_mgmt-acct"></a>

Follow these recommendations to help protect the security of the master account in AWS Organizations\. These recommendations assume that you also adhere to the[ best practice of using the root user only for those tasks that truly require it](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html)\.

**Topics**
+ [Use a group email address for the master account's root user](#best-practices_mgmt-acct_email-address)
+ [Use a complex password for the root user](#best-practices_mgmt-acct_complex-password)
+ [Enable MFA for your root user credentials](#best-practices_mgmt-acct_mfa)
+ [Add a phone number to the account contact information](#best-practices_mgmt-acct_phone-number)
+ [Review and keep track of who has access](#best-practices_mgmt-acct_review-access)
+ [Document the processes for using the root user credentials](#best-practices_mgmt-acct_document-processes)
+ [Apply controls to monitor access to the root user credentials](#best-practices_mgmt-acct_monitor-access)

## Use a group email address for the master account's root user<a name="best-practices_mgmt-acct_email-address"></a>
+ Use an email address that is managed by your business\. Do not use a public email provider or one that is managed by a third party\.
+ Use an email address that forwards received messages directly to a list of senior business managers\. In the event that AWS needs to contact the owner of the account, for example, to confirm access, the email message is distributed to multiple parties\. This approach helps to reduce the risk of delays in responding, even if individuals are on vacation, out sick, or leave the business\.

## Use a complex password for the root user<a name="best-practices_mgmt-acct_complex-password"></a>
+ The security of your account's root user depends on the strength of its password\. We recommend that you use a password that is long, complex, and not used anywhere else\. Numerous password managers and complex password generation algorithms and tools can help you achieve these goals\.
+ If you are using a strong password, as described in the previous point, and you rarely access the root user, we recommend that you do ***not*** periodically change the password\. Changing the password more frequently than you use it increases the risk of compromise\.
+ Rely on your business' information security policy for managing long\-term storage and access to the root user password\. This approach might mean that you do any of the following:
  + Print the password, and store it in a safe\.
  + Split the password into pieces, and distribute the pieces to senior business managers\.
  + Store the password in a password manager system or tool under further controls and processes\. If you do use a password manager, we recommend that it be offline\. To avoid creating a circular dependency, do not store the root user password with tools that depend on AWS services that you sign in to with the protected account\. 

  Whatever method you choose, we recommend that the method is resilient and requires multiple actors to be involved to reduce collusion risks\.
+ Any access of the password or its storage location should be logged and monitored\. 

## Enable MFA for your root user credentials<a name="best-practices_mgmt-acct_mfa"></a>

For instructions on how to enable multi\-factor authentication \(MFA\), see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html)\.
+ Use a hardware\-based device that does not rely on a battery to generate the one\-time password \(OTP\)\. This approach helps ensure that the MFA is impossible to duplicate and isn't subject to battery fade risks while in long\-term storage\. 
  + If you do decide to use a battery\-based MFA, be sure to add processes to check the device periodically, and replace it when the expiry date approaches\.
  + Create a plan to handle the logistics of needing to maintain 24/7 access to the token in case it is needed\.
+ We strongly recommend that you ***don't*** re\-use that physical MFA for any other purpose than protecting this master account\. If you reuse the physical MFA, it can create both operational confusion and unnecessary exposure of the MFA\.
+ Store the MFA device according to your information security policy but ***not*** in the same place as the associated password for the root user\. Make sure that the process to access the password and the process to access the MFA require different access to different resources \(people, data, and tools\)\. 
+ Any access of the MFA device or its storage location should be logged and monitored\. 

## Add a phone number to the account contact information<a name="best-practices_mgmt-acct_phone-number"></a>
+ Although there are some credible attack vectors against landline, SIP, and mobile phone numbers, overall the risks are outweighed by the complexity of these vectors\. If you use this mechanism to recover root access, other factors are available to the AWS Support representative to manage these risks\. Therefore, we recommend adding a phone number as a useful additional barrier to the process\.
+ There are several options for provisioning a phone number, but the one we recommend is a dedicated SIM card and phone, stored long\-term in a safe\. It’s important to ensure that the team responsible for paying the mobile bill for this phone contract understand the importance of the number even though there will be apparently no calls sent or received by it over long periods of time\.
+ It’s important that this phone number not be well known within the business\. Document it in the **AWS Contact Informatio**n console page, and share its details with your billing team\. Do not document it anywhere else\. This approach helps to reduce the risk of the attack vectors associated with moving the phone number tied to the SIM to another SIM\.
+ Store the phone according to your existing information security policy\. However, do not store the phone in the same location as the other related credential information\.
+ Any access of the phone or its storage location should be logged and monitored\.

## Review and keep track of who has access<a name="best-practices_mgmt-acct_review-access"></a>
+ To ensure you maintain access to the master account, periodically review the personnel within your business who have access to the email address, password, MFA, and phone number associated with it\. Align your review with existing business procedures\. However, it’s worth adding a monthly or quarterly review of this information to ensure that only the correct people have access\.
+ Ensure that the process to recover or reset access to the root user credentials is not reliant on any specific individual to complete\. All processes should address the prospect of people being unavailable\.
+ Ensure that the process to recover or reset access to the root user credentials can't be completed by a single individual\. It’s important that this process require the cooperation of multiple personnel to complete\.

## Document the processes for using the root user credentials<a name="best-practices_mgmt-acct_document-processes"></a>
+ It’s common for important processes, such as the creation of the organization's master account, to be a planned process including multiple steps with multiple personnel\. We recommend that you document and publish that plan, including the steps to be performed and their sequence of completion\. This approach helps ensure that the decisions made are followed correctly\.
+ Document the performance of important processes as they are performed to ensure you have a record of the individuals involved in each step and the values used\. It’s also important to provide documentation about any exceptions and unforeseen events that occur\.

  If an exception or unforeseen event does occur, document the time it occurred, who left the room, and what was taken out\. You should then also document who returned to the room and what was brought back in\. 
+ Create a suite of published processes about how to use the root user credentials in different scenarios, such as resetting the password\. If you are at all unsure about the process for interacting with AWS Support in a specific scenario, create a support ticket to ask for the latest guidance about how to perform that task\.

  Some scenarios you should document include the following:
  + Accessing the root user to perform one of the operations that only the root user can perform\. 
  + Resetting a root user password when you lose access\.
  + Changing the root user password when you still have access\.
  + Resetting the root user MFA when you lose access to the device\.
  + Changing the root user MFA when you use a battery\-based device\.
  + Resetting the root user email address when you lose access to the email account\.
  + Changing the root user email address when you still have access\.
  + Resetting the root user phone number when you lose access to the phone number\.
  + Changing the root user phone number when you still have access\.
  + Deleting the organization's master account\.
+ Test and validate that you continue to have access to the root user and that the mobile phone number is operational on at least a quarterly basis\. This schedule helps to assure the business that the process works and that you maintain access\. It also demonstrates that those custodians of the access understand the steps they need to perform for the process to succeed\. You never want to be in a position where the personnel involved in a process don’t understand what they are supposed to do\. As with fire drills, practice develops competency and reduces surprises\.

  With each test, take the opportunity to reflect on the experience and propose improvements to the process\. Especially examine any steps that were performed incorrectly or resulted in unexpected results\. How could you change the process to improve it the next time?

  Some customers use these tests as an opportunity to rotate passwords\. Our recommendation is not to rotate passwords\. Instead, maintain the same complex password\. You should only consider updating the password if you suspect it was compromised\.

## Apply controls to monitor access to the root user credentials<a name="best-practices_mgmt-acct_monitor-access"></a>
+ Access to the root user credentials should be a rare event\. Create alerts using tools like Amazon CloudWatch Events to announce the login and use of the master account root user credentials\. This announcement should include, but should not be limited to, the email address used for the root user itself\. This announcement should be significant and hard to miss, whether the use is valid or malicious\. For an example, see [Monitor and notify on AWS account root user activity](https://aws.amazon.com/blogs/mt/monitor-and-notify-on-aws-account-root-user-activity/)\.
+ Ensure that personnel who receive such an announcement understand how to validate that the root user access is expected, and how to escalate if they believe that a security incident is in progress\.