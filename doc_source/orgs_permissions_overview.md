# Overview of Managing Access Permissions to Your AWS Organization<a name="orgs_permissions_overview"></a>

All AWS resources, including the roots, organizational units, accounts, and policies in an organization, are owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. In the case of an organization, all resources are owned by the organization's master account\. An account administrator can control access to AWS resources by attaching permissions policies to IAM identities \(users, groups, and roles\)\.

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator permissions\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

By default, IAM users, groups, and roles have no permissions\. As an admin in the master account of an organization, you can perform administrative tasks or delegate admin permissions to other IAM users or roles in the master account\. To do this, you attach an IAM permissions policy to an IAM user, group, or role\. By default, a user has no permissions at all; this is sometimes called an *implicit deny*\. The policy overrides the implicit deny with an *explicit allow* that specifies which actions the user can perform, and which resources they can perform the actions on\. If the permissions are granted to a role, that role can be assumed by users in other accounts in the organization\.

## AWS Organizations Resources and Operations<a name="orgs-access-control-resources-and-operataions"></a>

This section discusses how Organizations concepts map to their IAM equivalent concepts\.

### Resources<a name="orgs_permissions_resources"></a>

In AWS Organizations, the resources to which you can control access include the root and the organizational units \(OUs\) that make up the hierarchical structure of an organization, the accounts that are members of the organization, the policies that you attach to the entities in the organization, and the handshakes that you use to change the state of the organization\.

Each of those resources has a unique Amazon Resource Name \(ARN\) associated with it\. You control access to a resource by specifying its ARN in the `Resource` element of an IAM permission policy\. For a complete list of the ARN formats for resources that are used in AWS Organizations, see [ARN Formats Supported by AWS Organizations](orgs_reference_arn-formats.md)\.

### Operations<a name="orgs_permissions_operations"></a>

AWS also provides a set of operations to work with the resources in an organization\. They enable you to do things like create, list, modify, access the contents of, or delete resources\. Most operations can be referenced in the `Action` element of an IAM policy to control who can use that operation\. For a list of Organizations operations that can be used as permissions in an IAM policy, see [Permissions You Can Use in an IAM Policy for AWS Organizations](orgs_reference_iam-permissions.md)\.

When you combine both an `Action` and a `Resource` in a single permission policy `Statement`, then you control exactly which resources that particular set of actions can be used on\.

## Understanding Resource Ownership<a name="orgs-access-control-resource-ownership"></a>

The AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the root account, an IAM user, or an IAM role\) that authenticates the resource creation request\. In the case of an AWS Organization, that is ***always*** the master account\. You cannot call most operations that create or access organization resources from the member accounts\. The following examples illustrate how this works:

+ If you use the root account credentials of your master account to create an organizational unit \(OU\), your master account is the owner of the resource \(in AWS Organizations, the resource is the OU\)\.

+ If you create an IAM user in your master account and grant permissions to create an OU to that user, the user can create an OU\. However, the master account, to which the user belongs, owns the OU resource\.

+ If you create an IAM role in your master account with permissions to create an OU, anyone who can assume the role can create an OU\. The master account, to which the role \(not the assuming user\) belongs, owns the OU resource\.

## Managing Access to Resources<a name="orgs-access-control-manage-access-to-resources"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of AWS Organizations\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see the [IAM User Guide](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. For information about IAM policy syntax and descriptions, see the [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies that are attached to an IAM identity are referred to as *identity\-based* policies \(IAM policies\)\. Policies that are attached to a resource are referred to as *resource\-based* policies\. AWS Organizations supports only identity\-based policies \(IAM policies\)\.


+ [Identity\-based Policies \(IAM Policies\)](#orgs-access-control-iam-policies)
+ [Resource\-based Policies](#orgs-access-control-resource-policies)

### Identity\-based Policies \(IAM Policies\)<a name="orgs-access-control-iam-policies"></a>

You can attach policies to IAM identities\. For example, you can do the following:

+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to create an AWS Organizations resource, such as a service control policy \(SCP\) or an organizational unit \(OU\), you can attach a permissions policy to a user or group that the user belongs to\. The user or group must be in the organization's master account\.

+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account access to an organization\. For example, the administrator in the master account can create a role to grant cross\-account permissions to a user in a member account as follows:

  1. The master account administrator creates an IAM role and attaches a permissions policy to the role that grants permissions to the organization's resources\.

  1. The master account administrator attaches a trust policy to the role that identifies the member account ID as the `Principal` who can assume the role\.

  1. The member account administrator can then delegate permissions to assume the role to any users in the member account\. Doing this allows users in the member account to create or access resources in the master account and the organization\. The principal in the trust policy can also be an AWS service principal if you want to grant permissions to an AWS service to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

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
```

For more information about using identity\-based policies, see [Using Identity\-Based Policies \(IAM Policies\) for AWS Organizations](orgs_permissions_iam-policies.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\.

### Resource\-based Policies<a name="orgs-access-control-resource-policies"></a>

Some services, such as Amazon S3, support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. AWS Organizations currently doesn't support resource\-based policies\.

## Specifying Policy Elements: Actions, Effects, and Resources<a name="orgs-access-control-policy-elements"></a>

For each AWS Organizations resource, the service defines a set of API operations, or actions, that can interact with or manipulate that resource in some way\. To grant permissions for these operations, Organizations defines a set of actions that you can specify in a policy\. For example, for the organizational unit \(OU\) resource, Organizations defines actions like the following:

+ `AttachPolicy` and `DetachPolicy`

+ `CreateOrganizationalUnit` and `DeleteOrganizationalUnit`

+ `ListOrganizationalUnits` and `DescribeOrganizationalUnit`

Note that, in some instances, performing an API operation might require permissions to more than one action, and might require permissions to more than one resource\.

The following are the most basic elements that you can use in an IAM permission policy:

+ **Action** – You use this keyword to identify the operations \(actions\) that you want to allow or deny\. For example, depending on the specified `Effect`, `organizations:CreateAccount` either allows or denies the user permissions to perform the AWS Organizations `CreateAccount` operation\. For more information, see [Operations](#orgs_permissions_operations)\.

+ **Resource** – You use this keyword to specify the Amazon Resource Name \(ARN\) of the resource to which the policy statement applies\. For more information, see [Resources](#orgs_permissions_resources)\.

+ **Effect** – You use this keyword to specify whether the policy statement allows or denies the action on the resource\. If you don't explicitly grant access to \(or allow\) a resource, access is implicitly denied\. You also can explicitly deny access to a resource, which you might do to ensure that a user cannot perform the specified action on the specified resource, even if a different policy grants access\.

+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is automatically and implicitly the principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. AWS Organizations currently supports only identity\-based policies, not resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see the [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table that show all the AWS Organizations API actions that can be used in IAM policies, see [Using Identity\-Based Policies \(IAM Policies\) for AWS Organizations](orgs_permissions_iam-policies.md)\.