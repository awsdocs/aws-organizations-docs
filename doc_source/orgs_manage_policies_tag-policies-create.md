# Creating, updating, and deleting tag policies<a name="orgs_manage_policies_tag-policies-create"></a>

After you enable tag policies for your organization, you can create a tag policy\.

**Important**  
Untagged resources don’t appear as noncompliant in results\.

## Creating a tag policy<a name="create-tag-policy-procedure"></a>

**Minimum permissions**  
To create tag policies, you need permission to run the following action:  
`organizations:CreatePolicy`

------
#### [ AWS Management Console ]

You can create a tag policy in the AWS Management Console in one of two ways:
+ A visual editor that lets you choose options and generates the JSON policy text for you\.
+ A text editor that lets you directly create the JSON policy text yourself\. 

The visual editor makes the process easy, but it limits your flexibility\. It's a great way to create your first policies and get comfortable with using them\. After you understand how they work and have started to be limited by what the visual editor provides, you can add advanced features to your policies by editing the JSON policy text yourself\. The visual editor uses only the [@@assign value\-setting operator](orgs_manage_policies_inheritance_mgmt.md#value-setting-operators), and it doesn't provide any access to the [child control operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators)\. You can add the child control operators only if you manually edit the JSON policy text\.

**To create a tag policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Tag policies](https://console.aws.amazon.com/organizations/home/policies/tag-policy)** page in the console\.

1. On the **Tag policies** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a ****Policy name**** and an optional **Policy description**\.

1. \(Optional\) You can add one or more tags to the policy object itself\. These tags are not part of the policy\. To do this, choose **Add tag** and then enter a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\.

1. You can build the tag policy using the **Visual editor** as described in this procedure\. You can also type or paste a tag policy in the **JSON** tab\. For information about tag policy syntax, see [Tag policy syntax](orgs_manage_policies_example-tag-policies.md#tag-policy-syntax-reference)\.

   For **New tag key 1**, specify the name of a tag key to add\. 

1. For **Tag key capitalization compliance**, leave this option cleared \(the default\) to specify that the inherited parent tag policy \(if one exists\) should define the case treatment for the tag key\. 

   Enable this option if you want to mandate a specific capitalization for the tag key\. If you select this option, the capitalization you specified for **Tag Key** overrides the case treatment specified in the inherited parent policy\. 

   If a parent policy doesn't exist and you don't select this option, tag keys in all lowercase characters are considered compliant\. For more information about parent policies, see [Understanding policy inheritance](orgs_manage_policies_inheritance.md)\.
**Tip**  
Consider using the example tag policy shown in [Example 1: Define organization\-wide tag key case](orgs_manage_policies_example-tag-policies.md#tag-policy-example-key-case) as a guide in creating a tag policy that define tag keys and their case treatment\. Attach it to the organization root\. Later, you can create and attach additional tag policies to OUs or accounts to create additional tagging rules\. 

1. For **Tag value compliance**, select the check box if you want to add allowed values for this tag key to any values inherited from a parent policy\.

   By default, this option is cleared, which means that only those values defined in and inherited from a parent policy are considered compliant\. If a parent policy doesn't exist and you don't specify tag values then any value \(including no value at all\) is considered compliant\. 

   To update the list of acceptable tag values, select **Specify allowed values for this tag key** and then choose **Specify values**\. When prompted, enter the new values \(one value per box\), and then choose **Save changes**\.

1. For **Prevent noncompliant operations for this tag**, we recommend that you leave this option cleared \(the default\) unless you are experienced with using tag policies\. Make sure that you have reviewed the recommendations in [Understanding enforcement](orgs_manage_policies_tag-policies-enforcement.md), and test thoroughly\. Otherwise, you could prevent users in your organization's accounts from tagging the resources they need\. 

   If you do want to enforce compliance with this tag key, select the check box and then **Specify resource types**\. When prompted, select the resource types to include in the policy\. Then choose **Save changes**\.
**Important**  
When you select this option, any operations that manipulate tags for resources of the specified types succeed only if the operation results in tags that are compliant with the policy\.

1. \(Optional\) To add another tag key to this tag policy, choose **Add tag key**\. Then perform steps 6–9 to define the tag key\.

1. When you're finished building your tag policy, choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To create a tag policy**  
You can use one of the following to create a tag policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  You can use any text editor to create a tag policy\. Use JSON syntax and save the tag policy as a file with any name and extension in a location of your choosing\. Tag policies can have a maximum of 2,500 characters, including spaces\. For information about tag policy syntax, see [Tag policy syntax](orgs_manage_policies_example-tag-policies.md#tag-policy-syntax-reference)\.

**To create a tag policy**

  1. Create a tag policy in a text file that looks similar to the following:

     Contents of `testpolicy.json`:

     ```
     {
         "tags": {
             "CostCenter": {
                 "tag_key": {
                     "@@assign": "CostCenter"
                 }
             }
         }
     }
     ```

     This tag policy defines the `CostCenter` tag key\. The tag can accept any value or no value\. A policy like this means that a resource that has the CostCenter tag attached with or without a value is compliant\.

  1. Create a policy that contains the policy content from the file\. Extra white space in the output has been truncated for readability\.

     ```
     $ aws organizations create-policy \
         --name "MyTestTagPolicy" \
         --description "My Test policy" \
         --content file://testpolicy.json \
         --type TAG_POLICY
     {
         "Policy": {
             "PolicySummary": {
                 "Id": "p-a1b2c3d4e5",
                 "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/tag_policy/p-a1b2c3d4e5",
                 "Name": "MyTestTagPolicy",
                 "Description": "My Test policy",
                 "Type": "TAG_POLICY",
                 "AwsManaged": false
             },
             "Content": "{\n\"tags\":{\n\"CostCenter\":{\n\"tag_key\":{\n\"@@assign\":\"CostCenter\"\n}\n}\n}\n}\n\n"
         }
     }
     ```
+ AWS SDKs: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

------

**What to Do Next**  
After you create a tag policy, you can put your tagging rules into effect\. To do that, [attach the policy ](attach-tag-policy.md) to the organization root, organizational units \(OUs\), AWS accounts within your organization, or a combination of organization entities\. 

## Updating a tag policy<a name="update-tag-policy-procedure"></a>

**Minimum permissions**  
To update a tag policy, you must have permission to run the following actions:  
`organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)

------
#### [ AWS Management Console ]

**To update a tag policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Tag policies](https://console.aws.amazon.com/organizations/home/policies/tag-policy)** page in the console\.

1. On the **Tag policies** page, choose the tag policy that you want to update\.

1. Choose **Edit policy**\.

1. You can enter a new **Policy name**, **Policy description**\. You can change the policy content by using either the **Visual editor** or by editing the **JSON**\. 

1. When you're finished updating the tag policy, choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To update a policy**  
You can use one of the following to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)

  The following example renames a tag policy\.

  ```
  $  aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --name "Renamed tag policy"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/tag_policy/p-i9j8k7l6m5",
              "Name": "Renamed tag policy",
              "Type": "TAG_POLICY",
              "AwsManaged": false
          },
          "Content": "{\n\"tags\":{\n\"CostCenter\":{\n\"tag_key\":{\n\"@@assign\":\"CostCenter\"\n}\n}\n}\n}\n\n"
      }
  }
  ```

  The following example adds or changes the description for a tag policy\.

  ```
  $  aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --description "My new tag policy description"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/tag_policy/p-i9j8k7l6m5",
              "Name": "Renamed tag policy",
              "Description": "My new tag policy description",
              "Type": "TAG_POLICY",
              "AwsManaged": false
          },
         "Content": "{\n\"tags\":{\n\"CostCenter\":{\n\"tag_key\":{\n\"@@assign\":\"CostCenter\"\n}\n}\n}\n}\n\n"
      }
  }
  ```

  The following example changes the JSON policy document attached to an AI services opt\-out policy\. In this example, the content is taken from a file called `policy.json` with the following text:

  ```
  {
    "tags": {
      "Stage": {
        "tag_key": {
          "@@assign": "Stage"
        },
        "tag_value": {
          "@@assign": [
            "Production",
            "Test"
          ]
        }
      }
    }
  }
  ```

  ```
  $ aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --content file://policy.json
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/tag_policy/p-i9j8k7l6m5",
              "Name": "Renamed tag policy",
              "Description": "My new tag policy description",
              "Type": "TAG_POLICY",
              "AwsManaged": false
          },
           "Content": "{\"tags\":{\"Stage\":{\"tag_key\":{\"@@assign\":\"Stage\"},\"tag_value\":{\"@@assign\":[\"Production\",\"Test\"]},\"enforced_for\":{\"@@assign\":[\"ec2:instance\"]}}}}"
  }
  ```
+ AWS SDKs: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

------

## Editing tags attached to a tag policy<a name="tag_tag-policies"></a>

When signed in to your organization's management account, you can add or remove the tags attached to a tag policy\. To do this, complete the following steps\.

**Minimum permissions**  
To edit the tags attached to a tag policy in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` \(console only – to navigate to the policy\)
`organizations:DescribePolicy` \(console only – to navigate to the policy\)
`organizations:TagResource`
`organizations:UntagResource`

------
#### [ AWS Management Console ]

**To edit the tags attached to an AI services opt\-out policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Tag policies](https://console.aws.amazon.com/organizations/home/policies/tag-policy)** page in the console\.

1. Choose the name of the policy with the tags that you want to edit\.

1. On the chosen policy's detail page, choose the **Tags** tab, and then choose **Manage tags**\.

1. You can perform any of these actions on this page:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ AWS CLI & AWS SDKs ]

**To edit the tags attached to a tag policy**  
You can use one of the following commands to edit the tags attached to a tag policy:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------

## Deleting a tag policy<a name="delete-tag-policy-procedure"></a>

When signed in to your organization's management account, you can delete a policy that you no longer need in your organization\. 

Before you can delete a policy, you must first detach it from all attached entities\.

**Minimum permissions**  
To delete a tag policy, you must have permission to run the following action:  
`organizations:DeletePolicy`

------
#### [ AWS Management Console ]

**To delete a tag policy**

1. Sign in to the AWS Organizations console at [https://console\.aws\.amazon\.com/organizations/](https://console.aws.amazon.com/organizations/)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. Navigate to the **[Tag policies](https://console.aws.amazon.com/organizations/home/policies/tag-policy)** page in the console\.

1. Choose the policy that you want to delete\. 

1. You must first detach the policy that you want to delete from all roots, OUs, and accounts\. Choose the **Targets** tab, choose the radio button next to each root, OU, or account that is shown in the **Targets** list, and then choose **Detach**\. In the confirmation dialog box, choose **Detach**\.

1. Choose **Delete** at the top of the page\.

1. On the confirmation dialog box, enter the name of the policy, and then choose **Delete**\.

------
#### [ AWS CLI & AWS SDKs ]

**To delete a tag policy**  
You can use one of the following to delete a policy:
+ AWS CLI: [aws organizations delete\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-policy.html)

  The following example deletes the specified policy\. It works only if the policy is not attached to any root, OU, or account\.

  ```
  $ aws organizations delete-policy \
      --policy-id p-i9j8k7l6m5
  ```

  This command produces no output when successful\.
+ AWS SDKs: [DeletePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeletePolicy.html)

------