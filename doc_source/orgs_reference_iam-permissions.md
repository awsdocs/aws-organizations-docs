# Resources, Permissions, and Context Keys You Can Use in an IAM Policy for AWS Organizations<a name="orgs_reference_iam-permissions"></a>

## Resource types available for the "Resources" policy element<a name="orgs_reference_iam-resources"></a>

The following table shows the resource types whose ARNs you can specify in the `Resources` element of an IAM permissions policy\. Each of these can be specified in a statement with relevant `Actions` to restrict those actions to only a specified resource or resource type\.

### Account<a name="orgs_reference_account"></a>

**Description: **Represents an AWS account in the organization\. Specifying an Organizations account ARN in an IAM policy statement restricts the `Actions` in the statement to act on only that account\.

**ARN format: **arn:aws:organizations::*<masterAccountId>*:account/*o\-<organizationId>*/*<accountId>*

### Handshake<a name="orgs_reference_handshake"></a>

**Description: **A request and its response from the organization to an account to change a configuration setting\. You typically specify a partial ARN down to the *<handshakeType>* and replacing the ID with **/\*** to restrict the `Actions` in the statement to act on only handshakes of the specified type\.

**ARN format: **arn:aws:organizations::*<masterAccountId>*:handshake/*o\-<organizationId>*/*<handshakeType>*/*h\-<handshakeId>*

**Example of typical use:**arn:aws:organizations::*<masterAccountId>*:handshake/*o\-<organizationId>*/*<handshakeType>*/\*

### Organization<a name="orgs_reference_organization"></a>

**Description: **A collection of AWS accounts that share billing and can be centrally managed using the AWS Organizations service\. Specifying the ARN of an organization restricts the `Actions` in the statement to act on only the specified organization\.

**ARN format: **arn:aws:organizations::*<masterAccountId>*:organization/*o\-<organizationId>*

### Organizational unit \(OU\)<a name="orgs_reference_ou"></a>

**Description: **A container of accounts to which separate organization controls can be applied\. Specifying the ARN of an OU restricts the `Actions` in the statement to act only on the specified OU and, if applicable, the accounts and OUs contained by the specified OU\.

**ARN format: **arn:aws:organizations::*<masterAccountId>*:ou/*o\-<organizationId>*/*ou\-<organizationalUnitId>*

### Policy<a name="orgs_reference_policy"></a>

**Description: **A set of rules that specify what users and roles in the affected accounts can perform\. Specifying the ARN of a policy restricts the `Actions` in the statement on act only on the specified policy or, if the policy ID is replaced by /\*, only policies of the specified type\.

**ARN format: **arn:aws:organizations::*<masterAccountId>*:policy/*o\-<organizationId>*/*<policyType>*/*p\-<policyId>*

**Example of typical use:**arn:aws:organizations::*<masterAccountId>*:policy/*o\-<organizationId>*/*<policyType>*/\*

### Root<a name="orgs_reference_root"></a>

**Description: **The top level container for all accounts and OUs\. Individual policy types can be enabled and disabled on a per\-root basis for the OUs within that root\. Specifying the ARN of a root restricts the `Actions` in a statement to act only on the specified root or, if applicable, the OUs and accounts in that root\.

**ARN format: **arn:aws:organizations::*<masterAccountId>*:root/*o\-<organizationId>*/*r\-<rootId>*

## Permissions for the "Actions" Element of an IAM Policy<a name="orgs_reference_iam-actions"></a>

For a complete list of the Organizations actions/permissions that you can specify in an IAM permissions policy, see the following topic:

[Actions Defined by AWS Organizations](http://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-actions-as-permissions) in the *IAM User Guide*\. 

The table there shows each action and which resources you can specify in a policy statement with that action\.

All actions can use `"Resource" : "*"`, but you also can specify ARNs of the types that are shown in the table to restrict access to only resources that match the specified ARNs\. 

## Resources for the "Resources" Element of an IAM Policy<a name="orgs_reference_iam-resources"></a>

All actions can use `"Resource" : "*"`\. However, many of the actions enable you to restrict the listed actions to a specific resource by including the resource's ARN in the `Resources` element of the policy statement\. For a list of all of the resource types that you can use with Organizations and the format of the ARN for each type, see the following topic:

[Resources Defined by AWS Organizations](http://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-resources-for-iam-policies) in the *IAM User Guide*\.

## Condition Keys for the "Conditions" Element of an IAM Policy<a name="orgs_reference_iam-context-keys"></a>

For a list of all of the condition keys that you can use in policies with Organizations, see the following topic:

[Condition Keys for AWS Organizations](http://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-policy-keys) in the *IAM User Guide*\.

The following keys can be used in IAM policies to provide more granular control over when the related action can be used\.
+ `organizations:ServicePrincipal` – You can use this key in a statement that has an `Action` element with `EnableAWSServiceAccess` or `DisableAWSServiceAccess`\. You can use this key to limit the scope of the permitted or denied actions to only the specified service principal\. For example, the condition in the following policy fragment grants permissions to the user to enable or disable trusted service access for only the service AWS SSO:

  ```
  "Effect": "Allow",
  "Action": [ "organizations:EnableAWSServiceAccess", "organizations:DisableAWSServiceAccess" ],
  "Condition": {
      "StringEquals": {
          "organizations:ServicePrincipal" : "sso.amazonaws.com"
      }
  }
  ```