# AWS managed policies available for use with AWS Organizations<a name="orgs_reference_available-policies"></a>

This section identifies the AWS\-managed policies provided for your use to manage your administration\. You can't modify or delete an AWS managed policy, but you can attach or detach them to entities in your organization as needed\.

## AWS Organizations managed service control policies<a name="ref-managed-policies"></a>

[Service control policies \(SCPs\)](orgs_manage_policies_scps.md) are similar to IAM permission policies, but are a feature of AWS Organizations rather than IAM\. You use SCPs to specify maximum permissions for affected entities\. You can attach SCPs to roots, organizational units \(OUs\), or accounts in your organization\. You can create your own, or you can use the policies that IAM defines\. You can see the list of policies in your organization on the [Policies](https://console.aws.amazon.com/organizations/?#/policies) page on the Organizations console\.

**Important**  
Every root, OU, and account must have at least one SCP attached at all times\.


****  

| Policy name | Description | ARN | 
| --- | --- | --- | 
| [FullAWSAccess](https://console.aws.amazon.com/organizations/?#/policies/p-FullAWSAccess) | Provides AWS Organizations master account access to member accounts\. | arn:aws:iam::aws:policy/AWSFullAccess | 