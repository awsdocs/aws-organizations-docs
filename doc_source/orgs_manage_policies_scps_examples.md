# Example service control policies<a name="orgs_manage_policies_scps_examples"></a>

The example [service control policies \(SCPs\)](orgs_manage_policies_scps.md) displayed in this topic are for information purposes only\.

**Before using these examples**  
Before you use these example SCPs in your organization, do the following:  
Carefully review and customize the SCPs for your unique requirements\.
Thoroughly test the SCPs in your environment with the AWS services that you use\.   
The example policies in this section demonstrate the implementation and use of SCPs\. They're ***not*** intended to be interpreted as official AWS recommendations or best practices to be implemented exactly as shown\. It is your responsibility to carefully test any deny\-based policies for its suitability to solve the business requirements of your environment\. Deny\-based service control policies can unintentionally limit or block your use of AWS services unless you add the necessary exceptions to the policy\. For an example of such an exception, see the first example that exempts global services from the rules that block access to unwanted AWS Regions\. 
Remember that an SCP affects every user and role in every account that it's attached to\. 

**Tip**  
You can use [service last accessed data](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) in [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) to update your SCPs to restrict access to only the AWS services that you need\. For more information, see [Viewing Organizations Service Last Accessed Data for Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor-view-data-orgs.html) in the *IAM User Guide\.* 

Each of the following policies is an example of a [deny list policy](orgs_manage_policies_scps_strategies.md#orgs_policies_denylist) strategy\. Deny list policies must be attached along with other policies that allow the approved actions in the affected accounts\. For example, the default `FullAWSAccess` policy permits the use of all services in an account\. This policy is attached by default to the root, all organizational units \(OUs\), and all accounts\. It doesn't actually grant the permissions; no SCP does\. Instead, it enables administrators in that account to delegate access to those actions by attaching standard AWS Identity and Access Management \(IAM\) permissions policies to users, roles, or groups in the account\. Each of these deny list policies then overrides any policy by blocking access to the specified services or actions\.

****Examples****
+ [General examples](orgs_manage_policies_scps_examples_general.md)
  + [Deny access to AWS based on the requested AWS Region](orgs_manage_policies_scps_examples_general.md#example-scp-deny-region)
  + [Prevent IAM users and roles from making certain changes](orgs_manage_policies_scps_examples_general.md#example-scp-restricts-iam-principals)
  + [Prevent IAM users and roles from making specified changes, with an exception for a specified admin role](orgs_manage_policies_scps_examples_general.md#example-scp-restricts-with-exception)
  + [Require MFA to perform an API action](orgs_manage_policies_scps_examples_general.md#example-scp-mfa)
  + [Block service access for the root user](orgs_manage_policies_scps_examples_general.md#example-scp-root-user)
  + [Prevent member accounts from leaving the organization](orgs_manage_policies_scps_examples_general.md#example-scp-leave-org)
+ [Example SCPs for Amazon CloudWatch](orgs_manage_policies_scps_examples_cloudwatch.md)
  + [Prevent users from disabling CloudWatch or altering its configuration](orgs_manage_policies_scps_examples_cloudwatch.md#example_cloudwatch_1)
+ [Example SCPs for AWS Config](orgs_manage_policies_scps_examples_config.md)
  + [Prevent users from disabling AWS Config or changing its rules](orgs_manage_policies_scps_examples_config.md#example_config_1)
+ [Example SCPs for Amazon Elastic Compute Cloud \(Amazon EC2\)](orgs_manage_policies_scps_examples_ec2.md)
  + [Require Amazon EC2 instances to use a specific type](orgs_manage_policies_scps_examples_ec2.md#example-ec2-1)
+ [Example SCPs for Amazon GuardDuty](orgs_manage_policies_scps_examples_guardduty.md)
  + [Prevent users from disabling GuardDuty or modifying its configuration](orgs_manage_policies_scps_examples_guardduty.md#example_guardduty_1)
+ [Example SCPs for AWS Resource Access Manager](orgs_manage_policies_scps_examples_ram.md)
  + [Preventing external sharing](orgs_manage_policies_scps_examples_ram.md#example_ram_1)
  + [Allowing specific accounts to share only specified resource types](orgs_manage_policies_scps_examples_ram.md#example_ram_2)
  + [Prevent sharing with organizations or organizational units \(OUs\)](orgs_manage_policies_scps_examples_ram.md#example_ram_3)
  + [Allow sharing with only specified IAM users and roles](orgs_manage_policies_scps_examples_ram.md#example_ram_4)
+ [Example SCPs for tagging resources](orgs_manage_policies_scps_examples_tagging.md)
  + [Require a tag on specified created resources](orgs_manage_policies_scps_examples_tagging.md#example-require-tag-on-create)
  + [Prevent tags from being modified except by authorized principals](orgs_manage_policies_scps_examples_tagging.md#example-require-restrict-tag-mods-to-admin)
+ [Example SCPs for Amazon Virtual Private Cloud \(Amazon VPC\)](orgs_manage_policies_scps_examples_vpc.md)
  + [Prevent users from deleting Amazon VPC flow logs](orgs_manage_policies_scps_examples_vpc.md#example_vpc_1)
  + [Prevent any VPC that doesn't already have internet access from getting it](orgs_manage_policies_scps_examples_vpc.md#example_vpc_2)