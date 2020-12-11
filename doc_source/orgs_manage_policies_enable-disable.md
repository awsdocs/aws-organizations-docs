# Enabling and disabling policy types<a name="orgs_manage_policies_enable-disable"></a>

## Enabling a policy type<a name="enable-policy-type"></a>

Before you can create and attach a policy to your organization, you must enable that policy type for use\. Enabling a policy type is a one\-time task on the organization root\. You can enable a policy type from only the organization's management account\.

**Minimum permissions**  
To enable a policy type, you need permission to run the following actions:  
`organizations:EnablePolicyType`
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:ListRoots` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To enable a policy type**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Policies](https://console.aws.amazon.com/organizations/home/policies)** page in the console\.

1. Choose the name of the policy type that you want to enable\.

1. On the policy type page, choose **Enable *policy type***\.

   The page is replaced by a list of the available policies of the specified type\.

------
#### [ AWS CLI & AWS SDKs ]

**To enable a policy type**  
You can use one of the following commands to enable a policy type:
+ AWS CLI: [aws organizations enable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-policy-type.html)

  The following example shows how to enable backup policies for your organization\. Note that you must specify the ID of your organization's root\.

  ```
  $ aws organizations enable-policy-type \
      --root-id r-a1b2 \
      --policy-type BACKUP_POLICY
  {
      "Root": {
          "Id": "r-a1b2",
          "Arn": "arn:aws:organizations::123456789012:root/o-aa111bb222/r-a1b2",
          "Name": "Root",
          "PolicyTypes": [
              {
                  "Type": "BACKUP_POLICY",
                  "Status": "ENABLED"
              }
          ]
      }
  }
  ```

  The list of `PolicyTypes` in the output now includes the specified policy type with the `Status` of `ENABLED`\.
+ AWS SDKs: [EnablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnablePolicyType.html)

------

## Disabling a policy type<a name="disable-policy-type"></a>

If you no longer want to use a certain policy type in your organization, you can disable that type to prevent its accidental use\. You can disable a policy type from only the organization's management account\.

**Important**  
When you disable a policy type, all policies of the specified type are automatically detached from all entities in the organization root\. The policies are ***not*** deleted\.
\(Service control policy type only\) If you re\-enable the SCP policy type later, all entities in the organization root are initially attached to only the default `FullAWSAccess` SCP\. Attachments of SCPs to entities are lost when the SCPs are disabled in the organization\. If you later want to re\-enable SCPs, you must reattach them to the organization's root, OUs, and accounts, as appropriate\.

**Minimum permissions**  
To disable SCPs, you need permission to run the following actions:  
`organizations:DisablePolicyType`
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:ListRoots` – required only when using the Organizations console

------
#### [ AWS Management Console ]

**To disable a policy type**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Policies](https://console.aws.amazon.com/organizations/home/policies)** page in the console\.

1. Choose the name of the policy type that you want to disable\.

1. On the policy type page, choose **Disable *policy type***\.

   The list of available policies of the specified type disappears\.

------
#### [ AWS CLI & AWS SDKs ]

**To disable a policy type**  
You can use one of the following commands to disable a policy type:
+ AWS CLI: [aws organizations disable\-policy\-type](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-policy-type.html)

  The following example shows how to disable backup policies for your organization\. Note that you must specify the ID of your organization's root\.

  ```
  $ aws organizations disable-policy-type \
      --root-id r-a1b2 \
      --policy-type BACKUP_POLICY
  {
      "Root": {
          "Id": "r-a1b2",
          "Arn": "arn:aws:organizations::123456789012:root/o-aa111bb222/r-a1b2",
          "Name": "Root",
          "PolicyTypes": []
      }
  }
  ```

  The list of `PolicyTypes` in the output no longer includes the specified policy type\.
+ AWS SDKs: [DisablePolicyType](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisablePolicyType.html)

------