# Getting information about your organization's policies<a name="orgs_manage_policies_info-operations"></a>

This section describes various ways to get details about the policies in your organization\. These procedures apply to *all* policy types\. You must enable a policy type on the organization root before you can attach policies of that type to any entities in that organization root\. 

## Listing all policies<a name="list-all-pols-in-org"></a>

**Minimum permissions**  
To list the policies within your organization, you must have the following permission:  
`organizations:ListPolicies`<a name="proc-list-all-pols-in-org"></a>

**To list all policies in your organization \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an AWS Identity and Access Management \(IAM\) user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. Choose the **Policies** tab\.

1.  Choose the policy type: **Service control policies** or **Tag policies**\. 

   The displayed list includes all policies of that type that are currently defined in the organization\.

**To list all policies in your organization \(AWS CLI, AWS API\)**  
You can use one of the following commands to list policies in an organization:
+ AWS CLI: [aws organizations list\-policies](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies.html)
+ AWS API: [ListPolicies](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPolicies.html)

## Listing all policies attached to a root, OU, or account<a name="list-all-pols-in-entity"></a>

**Minimum permissions**  
To list the policies that are attached to a root, organizational unit \(OU\), or account within your organization, you must have the following permission:  
`organizations:ListPoliciesForTarget` with a `Resource` element in the same policy statement that includes the Amazon Resource Name \(ARN\) of the specified target \(or "\*"\)

**To list all policies that are attached directly to a specified root, OU, or account \(console\)**

1. Sign in to the Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization's master account\.

1. On the **Organize accounts** tab, [navigate](orgs_manage_ous.md#navigate_tree) to the root, OU, or account whose policy attachments you want to see\.

   1. For a root or OU, don't select any check boxes\. This way, the details pane on the right shows the information about the root or OU that you are viewing\. Alternatively, you can navigate to the parent of the OU, and then select the check box for the OU whose information you want to see\.

   1. For an account, check the box for the account\.

1. In the details pane on the right, expand the **Service control policies** or **Tag policies** section\.

   The displayed list shows all policies of that type that are attached directly to this entity\. It also shows policies that affect this entity because of inheritance from the root or a parent OU\.

**To list all policies that are attached directly to a specified root, OU, or account \(AWS CLI, AWS API\)**  
You can use one of the following commands to list policies that are attached to an entity:
+ AWS CLI: [aws organizations list\-policies\-for\-target](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-policies-for-target.html)
+ AWS API: [ListPoliciesForTarget](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListPoliciesForTarget.html)

## Listing all roots, OUs, and accounts that a policy is attached to<a name="list-all-entities-attached-to-pol"></a>

**Minimum permissions**  
To list the entities that a policy is attached to, you must have the following permission:  
`organizations:ListTargetsForPolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

**To list all roots, OUs, and accounts that have a specified policy attached \(console\)**

1. Choose the **Policies** tab\.

1. Choose the policy type: **Service control polices** or **Tag policies**\.

1. Select the check box next to the policy that you're interested in\.

1. In the details pane on the right, choose one of the following:
   +  **Accounts** to see the list of accounts that the policy is directly attached to
   + **Organizational units** to see the list of OUs that the policy is directly attached to
   + **Roots** to see the list of roots that the policy is directly attached to

**To list all roots, OUs, and accounts that have a specified policy attached \(AWS CLI, AWS API\)**  
You can use one of the following commands to list entities that have a policy:
+ AWS CLI: [aws organizations list\-targets\-for\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-targets-for-policy.html)
+ AWS API: [ListTargetsForPolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_ListTargetsForPolicy.html)

## Getting details about a policy<a name="get-details-about-pol"></a>

**Minimum permissions**  
To display the details of a policy, you must have the following permission:  
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

**To get details about a policy \(console\)**

1. Choose the **Policies** tab\.

1. Choose the policy type: **Service control policies** or **Tag policies**\.

1. Select the check box next to the policy that you're interested in\.

   The details pane on the right displays the available information about the policy, including its ARN, description, and attachments\.

1. To view the content of the policy, choose **Policy editor**\.

   The center pane shows the following information:
   + The details about the policy: its name, description, unique ID, and ARN\.
   + The list of roots, OUs, and accounts that the policy is attached to\. Choose each item to see the individual entities of each type\.
   + The policy's content \(specific to the type of policy\):
     + For SCPs, the JSON text that defines the permissions that are allowed in attached accounts\.
     + For tag policies, the JSON text that defines compliant tags for specified resource types\.

     To update the contents of the policy document, choose **Edit**\. Choose **Save** when you are done\. For more details, see the next section\.

**To get details about a policy \(AWS CLI, AWS API\)**  
You can use one of the following commands to get details about a policy:
+ AWS CLI: [aws organizations describe\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/describe-policy.html)
+ AWS API: [DescribePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DescribePolicy.html)