# What is AWS Organizations?<a name="orgs_introduction"></a>

AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an *organization* that you create and centrally manage\. AWS Organizations includes account management and consolidated billing capabilities that enable you to better meet the budgetary, security, and compliance needs of your business\. As an administrator of an organization, you can create accounts in your organization and invite existing accounts to join the organization\. 

This user guide defines [ key concepts for AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html), provides [ tutorials](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_tutorials.html), and explains how to [ create and manage an organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org.html)\. 

**Topics**
+ [AWS Organizations features](#features)
+ [AWS Organizations pricing](#pricing)
+ [Accessing AWS Organizations](#how-to-access)
+ [Support and feedback for AWS Organizations](#support-and-feedback)
+ [AWS services that you can use with AWS Organizations](orgs_integrated-services-list.md)

## AWS Organizations features<a name="features"></a>

AWS Organizations offers the following features:

**Centralized management of all of your AWS accounts**  
You can combine your existing accounts into an organization that enables you to manage the accounts centrally\. You can create accounts that automatically are a part of your organization, and you can invite other accounts to join your organization\. You also can attach policies that affect some or all of your accounts\. 

**Consolidated billing for all member accounts**  
Consolidated billing is a feature of AWS Organizations\. You can use the master account of your organization to consolidate and pay for all member accounts\. In consolidated billing, master accounts can also access the billing information, account information, and account activity of member accounts in their organization\. This information may be used for services such as Cost Explorer, which can help master accounts improve their organization’s cost performance\. 

**Hierarchical grouping of your accounts to meet your budgetary, security, or compliance needs**  
You can group your accounts into organizational units \(OUs\) and attach different access policies to each OU\. For example, if you have accounts that must access only the AWS services that meet certain regulatory requirements, you can put those accounts into one OU\. You then can attach a policy to that OU that blocks access to services that do not meet those regulatory requirements\. You can nest OUs within other OUs to a depth of five levels, providing flexibility in how you structure your account groups\.

**Policies to centralize control over the AWS services and API actions that each account can access**  
As an administrator of the master account of an organization, you can use service control policies \(SCPs\) to specify the maximum permissions for member accounts in the organization\. In SCPs, you can restrict which AWS services, resources, and individual API actions the users and roles in each member account can access\. You can also define conditions for when to restrict access to AWS services, resources, and API actions\. These restrictions even override the administrators of member accounts in the organization\. When AWS Organizations blocks access to a service, resource, or API action for a member account, a user or role in that account can't access it\. This block remains in effect even if an administrator of a member account explicitly grants such permissions in an IAM policy\.   
For more information, see [Service control policies](orgs_manage_policies_scps.md)\. 

**Policies to standardize tags across the resources in your organization's accounts**  
You can use tag policies to maintain consistent tags, including the preferred case treatment of tag keys and tag values\.  
For more information, see [Tag policies](orgs_manage_policies_tag-policies.md)

**Policies to control how AWS artificial intelligence \(AI\) and machine learning services can collect and store data\.**  
You can use AI services opt\-out policies to opt out of data collection and storage for any of the AWS AI services that you don't want to use\.  
For more information, see [AI services opt\-out policies](orgs_manage_policies_ai-opt-out.md)

**Policies that configure automatic backups for the resources in your organization's accounts**  
You can use backup policies to configure and automatically apply AWS Backup plans to resources across all your organization's accounts\.  
For more information, see [Backup policies](orgs_manage_policies_backup.md)

**Integration and support for AWS Identity and Access Management \(IAM\)**  
[IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) provides granular control over users and roles in individual accounts\. AWS Organizations expands that control to the account level by giving you control over what users and roles in an account or a group of accounts can do\. The resulting permissions are the logical intersection of what is allowed by AWS Organizations at the account level and the permissions that are explicitly granted by IAM at the user or role level within that account\. In other words, the user can access only what is allowed by ***both*** the AWS Organizations policies and IAM policies\. If either blocks an operation, the user can't access that operation\.

**Integration with other AWS services**  
You can leverage the multi\-account management services available in AWS Organizations with select AWS services to perform tasks on all accounts that are members of an organization\. For a list of services and the benefits of using each service on an organization\-wide level, see [AWS services that you can use with AWS Organizations](orgs_integrated-services-list.md)\.   
When you enable an AWS service to perform tasks on your behalf in your organization's member accounts, AWS Organizations creates an [IAM service\-linked role](http://aws.amazon.com/blogs/security/introducing-an-easier-way-to-delegate-permissions-to-aws-services-service-linked-roles/) for that service in each member account\. The service\-linked role has predefined IAM permissions that allow the other AWS service to perform specific tasks in your organization and its accounts\. For this to work, all accounts in an organization automatically have a [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html?icmpid=docs_iam_console#iam-term-service-linked-role)\. This role must enable the AWS Organizations service to create the service\-linked roles required by AWS services for which you enable trusted access\. These additional service\-linked roles come with policies that enable the specified service to perform only those tasks that are required by your configuration choices\. For more information, see [Enabling trusted access with other AWS services](orgs_integrate_services.md)\.

**Global access**  
AWS Organizations is a global service with a single endpoint that works from any and all AWS Regions\. You don't need to explicitly select a region to operate in\. 

**Data replication that is eventually consistent**  
AWS Organizations, like many other AWS services, is [eventually consistent](https://wikipedia.org/wiki/Eventual_consistency)\. AWS Organizations achieves high availability by replicating data across multiple servers in AWS data centers within its Region\. If a request to change some data is successful, the change is committed and safely stored\. However, the change must then be replicated across the multiple servers\. For more information, see [Changes that I make aren't always immediately visible](orgs_troubleshoot_general.md#troubleshoot_general_eventual-consistency)\.

**Free to use**  
AWS Organizations is a feature of your AWS account offered at no additional charge\. You are charged only when you access other AWS services from the accounts in your organization\. For information about the pricing of other AWS products, see the [Amazon Web Services pricing page](http://aws.amazon.com/pricing/)\.

## AWS Organizations pricing<a name="pricing"></a>

AWS Organizations is offered at no additional charge\. You are charged only for AWS resources that users and roles in your member accounts use\. For example, you are charged the standard fees for Amazon EC2 instances that are used by users or roles in your member accounts\. For information about the pricing of other AWS services, see [AWS Pricing](https://aws.amazon.com/pricing/services/)\.

## Accessing AWS Organizations<a name="how-to-access"></a>

You can work with AWS Organizations in any of the following ways:

**AWS Management Console**  
[The AWS Organizations console](https://console.aws.amazon.com/organizations/) is a browser\-based interface that you can use to manage your organization and your AWS resources\. You can perform any task in your organization by using the console\.

**AWS Command Line Tools**  
With the AWS command line tools, you can issue commands at your system's command line to perform AWS Organizations and AWS tasks\. Working with the command line can be faster and more convenient than using the console\. The command line tools also are useful if you want to build scripts that perform AWS tasks\.  
AWS provides two sets of command line tools:  
+  [AWS Command Line Interface](https://aws.amazon.com/cli/) \(AWS CLI\)\. For information about installing and using the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.
+  [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\. For information about installing and using the Tools for Windows PowerShell, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\.

**AWS SDKs**  
The AWS SDKs consist of libraries and sample code for various programming languages and platforms \(for example, Java, Python, Ruby, \.NET, iOS, and Android\)\. The SDKs take care of tasks such as cryptographically signing requests, managing errors, and retrying requests automatically\. For more information about the AWS SDKs, including how to download and install them, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/#sdk)\.

**AWS Organizations HTTPS Query API**  
The AWS Organizations HTTPS Query API gives you programmatic access to AWS Organizations and AWS\. The HTTPS Query API lets you issue HTTPS requests directly to the service\. When you use the HTTPS API, you must include code to digitally sign requests using your credentials\. For more information, see [Calling the API by Making HTTP Query Requests](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_query-requests.html) and the [AWS Organizations API Reference](https://docs.aws.amazon.com/organizations/latest/APIReference/)\.

## Support and feedback for AWS Organizations<a name="support-and-feedback"></a>

We welcome your feedback\. You can send comments to [feedback\-awsorganizations@amazon\.com](mailto:feedback-awsorganizations@amazon.com)\. You also can post your feedback and questions in [AWS Organizations support forum](https://forums.aws.amazon.com/forum.jspa?forumID=219)\. For more information about the AWS Support forums, see [Forums Help](https://forums.aws.amazon.com/help.jspa)\.

### Other AWS resources<a name="other-resources"></a>
+ **[AWS Training and Courses](https://aws.amazon.com/training/course-descriptions/)** – Links to role\-based and specialty courses as well as self\-paced labs to help sharpen your AWS skills and gain practical experience\.
+ **[AWS Developer Tools](https://aws.amazon.com/developertools/)** – Links to developer tools and resources that provide documentation, code examples, release notes, and other information to help you build innovative applications with AWS\.
+ **[AWS Support Center](https://console.aws.amazon.com/support/home#/)** – The hub for creating and managing your AWS Support cases\. Also includes links to other helpful resources, such as forums, technical FAQs, service health status, and AWS Trusted Advisor\.
+ **[AWS Support](https://aws.amazon.com/premiumsupport/)** – The primary webpage for information about AWS Support, a one\-on\-one, fast\-response support channel to help you build and run applications in the cloud\.
+ **[Contact Us](https://aws.amazon.com/contact-us/)** – A central contact point for inquiries concerning AWS billing, account, events, abuse, and other issues\.
+ **[AWS Site Terms](https://aws.amazon.com/terms/)** – Detailed information about our copyright and trademark; your account, license, and site access; and other topics\.