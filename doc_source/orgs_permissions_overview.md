# Managing Access Permissions for Your AWS Organization<a name="orgs_permissions_overview"></a>

All AWS resources, including the roots, OUs, accounts, and policies in an organization, are owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. In the case of an organization, its master account owns all resources\. An account administrator can control access to AWS resources by attaching permissions policies to IAM identities \(users, groups, and roles\)\.

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator permissions\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, the resources that they get permissions for, and the specific actions that you want to allow on those resources\.

By default, IAM users, groups, and roles have no permissions\. As an administrator in the master account of an organization, you can perform administrative tasks or delegate administrator permissions to other IAM users or roles in the master account\. To do this, you attach an IAM permissions policy to an IAM user, group, or role\. By default, a user has no permissions at all; this is sometimes called an *implicit deny*\. The policy overrides the implicit deny with an *explicit allow* that specifies which actions the user can perform, and which resources they can perform the actions on\. If the permissions are granted to a role, users in other accounts in the organization can assume that role\.

## AWS Organizations Resources and Operations<a name="orgs-access-control-resources-and-operataions"></a>

This section discusses how AWS Organizations concepts map to their IAM\-equivalent concepts\.

### Resources<a name="orgs_permissions_resources"></a>

In AWS Organizations, you can control access to the following resources:
+ The root and the OUs that make up the hierarchical structure of an organization
+ The accounts that are members of the organization
+ The policies that you attach to the entities in the organization
+ The handshakes that you use to change the state of the organization

Each of these resources has a unique Amazon Resource Name \(ARN\) associated with it\. You control access to a resource by specifying its ARN in the `Resource` element of an IAM permission policy\. For a complete list of the ARN formats for resources that are used in AWS Organizations, see [Resources Defined by AWS Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-resources-for-iam-policies) in the *IAM User Guide*\.

### Operations<a name="orgs_permissions_operations"></a>

AWS provides a set of operations to work with the resources in an organization\. They enable you to do things like create, list, modify, access the contents of, and delete resources\. Most operations can be referenced in the `Action` element of an IAM policy to control who can use that operation\. For a list of AWS Organizations operations that can be used as permissions in an IAM policy, see [API Action Permissions Defined by AWS Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-actions-as-permissions) in the *IAM User Guide*\.

When you combine an `Action` and a `Resource` in a single permission policy `Statement`, you control exactly which resources that particular set of actions can be used on\.

### Condition Keys<a name="orgs_permissions_conditionkeys"></a>

AWS provides condition keys that you can query to provide more granular control over certain actions\. You can reference these condition keys in the `Condition` element of an IAM policy to specify the additional circumstances that must be met for the statement to be considered a match\. 

The following condition keys are especially useful with AWS Organizations:
+ `organizations:ServicePrincipal` – Available as a condition if you use the [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html) or [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html) operations to enable or disable [trusted access](orgs_integrate_services.md) with other AWS services\. You can use `organizations:ServicePrincipal` to limit requests that those operations make to a list of approved service principal names\.

  For example, the following policy allows the user to specify only AWS Firewall Manager when enabling and disabling trusted access with AWS Organizations: 

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AllowOnlyAWSFirewallIntegration",
              "Effect": "Allow",
              "Action": [
                  "organizations:EnableAWSServiceAccess",
                  "organizations:DisableAWSServiceAccess"
              ],
              "Resource": "*",
              "Condition": { 
                  "ForAllValues:StringLike": {
                      "organizations:ServicePrincipal": [ "fms.amazonaws.com" ]
                  }
              }
          }
      ]
  }
  ```
+ `aws:PrincipalOrgID` – Simplifies specifying the `Principal` element in a resource\-based policy\. This global key provides an alternative to listing all the account IDs for all AWS accounts in an organization\. Instead of listing all of the accounts that are members of an organization, you can specify the [organization ID](orgs_manage_org_details.md) in the `Condition` element\. 
**Note**  
This global condition also applies to the master account of an AWS organization\.

  For more information, see the description of `PrincipalOrgID` in [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

For a list of all of the AWS Organizations–specific condition keys that can be used as permissions in an IAM policy, see [Condition Context Keys for AWS Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-policy-keys) in the *IAM User Guide*\.

## Understanding Resource Ownership<a name="orgs-access-control-resource-ownership"></a>

The AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the root account, an IAM user, or an IAM role\) that authenticates the resource creation request\. For an AWS organization, that is ***always*** the master account\. You can't call most operations that create or access organization resources from the member accounts\. The following examples illustrate how this works:
+ If you use the root account credentials of your master account to create an OU, your master account is the owner of the resource\. \(In AWS Organizations, the resource is the OU\.\)
+ If you create an IAM user in your master account and grant permissions to create an OU to that user, the user can create an OU\. However, the master account, to which the user belongs, owns the OU resource\.
+ If you create an IAM role in your master account with permissions to create an OU, anyone who can assume the role can create an OU\. The master account, to which the role \(not the assuming user\) belongs, owns the OU resource\.

## Managing Access to Resources<a name="orgs-access-control-manage-access-to-resources"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of AWS Organizations\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. For information about IAM policy syntax and descriptions, see the [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies that are attached to an IAM identity are referred to as *identity\-based* policies \(IAM policies\)\. Policies that are attached to a resource are referred to as *resource\-based* policies\. AWS Organizations supports only identity\-based policies \(IAM policies\)\.

**Topics**
+ [Identity\-based Policies \(IAM Policies\)](#orgs-access-control-iam-policies)
+ [Resource\-based Policies](#orgs-access-control-resource-policies)

### Identity\-based Policies \(IAM Policies\)<a name="orgs-access-control-iam-policies"></a>

You can attach policies to IAM identities\. For example, you can do the following:
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to create an AWS Organizations resource, such as a service control policy \(SCP\) or an OU, you can attach a permissions policy to a user or a group that the user belongs to\. The user or group must be in the organization's master account\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account access to an organization\. For example, the administrator in the master account can create a role to grant cross\-account permissions to a user in a member account as follows:

  1. The master account administrator creates an IAM role and attaches a permissions policy to the role that grants permissions to the organization's resources\.

  1. The master account administrator attaches a trust policy to the role that identifies the member account ID as the `Principal` who can assume the role\.

  1. The member account administrator can then delegate permissions to assume the role to any users in the member account\. Doing this allows users in the member account to create or access resources in the master account and the organization\. The principal in the trust policy can also be an AWS service principal if you want to grant permissions to an AWS service to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following is an example policy that allows a user to perform the `CreateAccount` action in your organization:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid" : "Stmt1OrgPermissions",  
      "Effect": "Allow",
      "Action": [
        "organizations:CreateAccount"
      ],
      "Resource": "*"
      }
   ]
}
```

For more information about using identity\-based policies, see [Using Identity\-Based Policies \(IAM Policies\) for AWS Organizations](orgs_permissions_iam-policies.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\.

### Resource\-based Policies<a name="orgs-access-control-resource-policies"></a>

Some services, such as Amazon S3, support resource\-based permissions policies\. For example, you can attach a policy to an Amazon S3 bucket to manage access permissions to that bucket\. AWS Organizations currently doesn't support resource\-based policies\.

## Specifying Policy Elements: Actions, Conditions, Effects, and Resources<a name="orgs-access-control-policy-elements"></a>

For each AWS Organizations resource, the service defines a set of API operations, or actions, that can interact with or manipulate that resource in some way\. To grant permissions for these operations, AWS Organizations defines a set of actions that you can specify in a policy\. For example, for the OU resource, AWS Organizations defines actions like the following:
+ `AttachPolicy` and `DetachPolicy`
+ `CreateOrganizationalUnit` and `DeleteOrganizationalUnit`
+ `ListOrganizationalUnits` and `DescribeOrganizationalUnit`

In some cases, performing an API operation might require permissions to more than one action and might require permissions to more than one resource\.

The following are the most basic elements that you can use in an IAM permission policy:
+ **Action** – Use this keyword to identify the operations \(actions\) that you want to allow or deny\. For example, depending on the specified `Effect`, `organizations:CreateAccount` allows or denies the user permissions to perform the AWS Organizations `CreateAccount` operation\. For more information, see [IAM JSON Policy Elements: Action](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_action.html) in the *IAM User Guide*\.
+ **Resource** – Use this keyword to specify the ARN of the resource to which the policy statement applies\. For more information, see [IAM JSON Policy Elements: Resource](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_resource.html) in the *IAM User Guide*\.
+ **Condition** – Use this keyword to specify a condition that must be met for the policy statement to apply\. `Condition` usually specifies additional circumstances that must be true for the policy to match\. For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Effect** – Use this keyword to specify whether the policy statement allows or denies the action on the resource\. If you don't explicitly grant access to \(or allow\) a resource, access is implicitly denied\. You also can explicitly deny access to a resource, which you might do to ensure that a user can't perform the specified action on the specified resource, even if a different policy grants access\. For more information, see [IAM JSON Policy Elements: Effect](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_effect.html) in the *IAM User Guide*\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is automatically and implicitly the principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. AWS Organizations currently supports only identity\-based policies, not resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see the [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table that show all of the AWS Organizations API actions that can be used in IAM policies, see [Using Identity\-Based Policies \(IAM Policies\) for AWS Organizations](orgs_permissions_iam-policies.md)\.