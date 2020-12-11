# Getting information about your organization's policies<a name="orgs_manage_policies_info-operations"></a>

This section describes various ways to get details about the policies in your organization\. These procedures apply to *all* policy types\. You must enable a policy type on the organization root before you can attach policies of that type to any entities in that organization root\. 

## Listing all policies<a name="list-all-pols-in-org"></a>

**Minimum permissions**  
To list the policies within your organization, you must have the following permission:  
`organizations:ListPolicies`

You can view the policies in your organization in the AWS Management Console or by using an AWS Command Line Interface \(AWS CLI\) command or an AWS SDK operation\.

------
#### [ AWS Management Console ]<a name="proc-list-all-pols-in-org"></a>

**To list all of the policies in your organization**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Policies](https://console.aws.amazon.com/organizations/home/policies)** page in the console\.

1. Choose the policy type that you want to list\.

   If the specified policy type is enabled, the console displays a list of all of the policies of that type that are currently available in the organization\.

------
#### [ AWS CLI & AWS SDKs ]

**To list all of the policies in your organization**  
You can use one of the following commands to list policies in an organization:
+ AWS CLI: [aws organizations list\-policies](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies.html)

  The following example shows how to get a list of all of the service control policies in your organization\. You must specify the type of policy you want see\.

  ```
  $ aws organizations list-policies \
      --filter SERVICE_CONTROL_POLICY
  {
      "Policies": [
          {
              "Id": "p-FullAWSAccess",
              "Arn": "arn:aws:organizations::aws:policy/service_control_policy/p-FullAWSAccess",
              "Name": "FullAWSAccess",
              "Description": "Allows access to every operation",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": true
          }
      ]
  }
  ```
+ AWS SDKs: [ListPolicies](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPolicies.html)

------

## Listing the policies attached to a root, OU, or account<a name="list-all-pols-in-entity"></a>

**Minimum permissions**  
To list the policies that are attached to a root, organizational unit \(OU\), or account within your organization, you must have the following permission:  
`organizations:ListPoliciesForTarget` with a `Resource` element in the same policy statement that includes the Amazon Resource Name \(ARN\) of the specified target \(or "\*"\)

------
#### [ AWS Management Console ]

**To list all policies that are attached directly to a specified root, OU, or account**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/home/accounts)** page in the console\. 

1. Choose the name of the root, OU, or account whose policies you want to view\. You might have to expand OUs \(choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png) next to an OU's name to expand it\) to find the OU or account that you want\.

1. On the detail page for the root, OU, or account, choose the **Policies** tab\.

   The **Policies** tab displays all of the policies attached to that root, OU, or account, grouped by policy type\.

------
#### [ AWS CLI & AWS SDKs ]

**To list all policies that are attached directly to a specified root, OU, or account**  
You can use one of the following commands to list policies that are attached to an entity:
+ AWS CLI: [aws organizations list\-policies\-for\-target](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies-for-target.html)

  The following example lists all of the service control policies attached to the specified OU\. You must specify both the ID of the root, OU, or account, and the type of policy that you want to list\.

  ```
  $  aws organizations list-policies-for-target \
      --target-id ou-a1b2-f6g7h222 \
      --filter SERVICE_CONTROL_POLICY
  {
      "Policies": [
          {
              "Id": "p-FullAWSAccess",
              "Arn": "arn:aws:organizations::aws:policy/service_control_policy/p-FullAWSAccess",
              "Name": "FullAWSAccess",
              "Description": "Allows access to every operation",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": true
          }
      ]
  }
  ```
+ AWS SDKs: [ListPoliciesForTarget](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPoliciesForTarget.html)

------

## Listing all roots, OUs, and accounts that a policy is attached to<a name="list-all-entities-attached-to-pol"></a>

**Minimum permissions**  
To list the entities that a policy is attached to, you must have the following permission:  
`organizations:ListTargetsForPolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

------
#### [ AWS Management Console ]

**To list all roots, OUs, and accounts that have a specified policy attached**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Policies](https://console.aws.amazon.com/organizations/home/policies)** page in the console\.

1. Choose the policy type of the policy that you want to examine\.

1. Choose the policy name whose attachments you want to examine\.

1. Choose the **Targets** tab\.

   A table displays every root, OU, and account that the chosen policy is attached to\.

------
#### [ AWS CLI & AWS SDKs ]

**To list all roots, OUs, and accounts that have a specified policy attached**  
You can use one of the following commands to list entities that have a policy:
+ AWS CLI: [aws organizations list\-targets\-for\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-targets-for-policy.html)

  The following example shows all of the attachments to root, OUs, and accounts for the specified policy\.

  ```
  $ aws organizations list-targets-for-policy \
      --policy-id p-FullAWSAccess
  {
      "Targets": [
          {
              "TargetId": "ou-a1b2-f6g7h111",
              "Arn": "arn:aws:organizations::123456789012:ou/o-aa111bb222/ou-a1b2-f6g7h111",
              "Name": "testou2",
              "Type": "ORGANIZATIONAL_UNIT"
          },
          {
              "TargetId": "ou-a1b2-f6g7h222",
              "Arn": "arn:aws:organizations::123456789012:ou/o-aa111bb222/ou-a1b2-f6g7h222",
              "Name": "testou1",
              "Type": "ORGANIZATIONAL_UNIT"
          },
          {
              "TargetId": "123456789012",
              "Arn": "arn:aws:organizations::123456789012:account/o-aa111bb222/123456789012",
              "Name": "My Management Account (bisdavid)",
              "Type": "ACCOUNT"
          },
          {
              "TargetId": "r-a1b2",
              "Arn": "arn:aws:organizations::123456789012:root/o-aa111bb222/r-a1b2",
              "Name": "Root",
              "Type": "ROOT"
          }
      ]
  }
  ```
+ AWS SDKs: [ListTargetsForPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListTargetsForPolicy.html)

------

## Getting details about a policy<a name="get-details-about-pol"></a>

**Minimum permissions**  
To display the details of a policy, you must have the following permission:  
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

------
#### [ AWS Management Console ]

**To get details about a policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Policies](https://console.aws.amazon.com/organizations/home/policies)** page in the console\.

1. Choose the policy type for the policy you want to examine\.

1. Choose the name of the policy that you want to examine\.

   The detail page displays the available information about the policy, including its ARN, description, and attached targets\. The **Content** tab shows the current contents of the policy in JSON format, the **Targets** tab shows a list of the roots, OUs, and accounts to which the policy is attached, and the **Tags** shows the tags attached to the policy\.

------
#### [ AWS CLI & AWS SDKs ]

**To get details about a policy**  
You can use one of the following commands to get details about a policy:
+ AWS CLI: [aws organizations describe\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-policy.html)

  The following example displays the details for the specified policy\.

  ```
  $  aws organizations describe-policy \
      --policy-id p-FullAWSAccess
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-FullAWSAccess",
              "Arn": "arn:aws:organizations::aws:policy/service_control_policy/p-FullAWSAccess",
              "Name": "FullAWSAccess",
              "Description": "Allows access to every operation",
              "Type": "SERVICE_CONTROL_POLICY",
              "AwsManaged": true
          },
          "Content": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Effect\": \"Allow\",\n      \"Action\": \"*\",\n      \"Resource\": \"*\"\n    }\n  ]\n}"
      }
  }
  ```
+ AWS SDKs: [DescribePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribePolicy.html)

------