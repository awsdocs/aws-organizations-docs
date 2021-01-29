# Creating, updating, and deleting AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_create"></a>

**Note**  
AWS Organizations is introducing a new version of the Organizations management console\. You can switch between the old console and the new console by choosing the link in the notice boxes at the top of the console\. We encourage you to try the new version and let us know what you think\. We want your feedback and read each submission\.

**In this topic:**
+ After you [enable AI service opt\-out policies](orgs_manage_policies_enable-disable.md#enable-policy-type) for your organization, you can [create a policy](#create-ai-opt-out-policy-procedure)\.
+ When your opt\-out requirements change, you can [update an existing policy](#update-ai-opt-out-policy-procedure)\.
+ When you no longer need a policy and after you detach it from all organizational units \(OUs\) and accounts, you can [delete it](#delete-ai-opt-out-policy-procedure)\.

## Creating an AI services opt\-out policy<a name="create-ai-opt-out-policy-procedure"></a>

**Minimum permissions**  
To create an AI services opt\-out policy, you need permission to run the following action:  
`organizations:CreatePolicy`

------
#### [ Old console ]

**To create an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/home#/policies/ai-services)** page, choose **Create policy**\. 

1. On the **Create policy** page, enter a **Policy name** and an optional **Policy description**\.

1. \(Optional\)You can add one or more tags to the policy by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\. For more information, see [Tagging AWS Organizations resources](orgs_tagging.md)\.

1. Enter or paste the policy text in the **JSON** tab\. For information about AI services opt\-out policy syntax, see [AI services opt\-out policy syntax and examples](orgs_manage_policies_ai-opt-out_syntax.md)\. For example policies that you can use as a starting point, see [AI services opt\-out policy examples](orgs_manage_policies_ai-opt-out_syntax.md#ai-opt-out-policy-examples)\.

1. When you're finished editing your policy, choose **Create policy** at the lower\-right corner of the page\.

------
#### [ New console ]

**To create an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy)** page, choose **Create policy**\. 

1. On the [**Create new AI services opt\-out policy** page](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy/create), enter a **Policy name** and an optional **Policy description**\.

1. \(Optional\) You can add one or more tags to the policy by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to a policy\. For more information, see [Tagging AWS Organizations resources](orgs_tagging.md)\.

1. Enter or paste the policy text in the **JSON** tab\. For information about AI services opt\-out policy syntax, see [AI services opt\-out policy syntax and examples](orgs_manage_policies_ai-opt-out_syntax.md)\. For example policies that you can use as a starting point, see [AI services opt\-out policy examples](orgs_manage_policies_ai-opt-out_syntax.md#ai-opt-out-policy-examples)\.

1. When you're finished editing your policy, choose **Create policy** at the lower\-right corner of the page\.

------
#### [ AWS CLI & AWS SDKs ]

**To create an AI services opt\-out policy**  
You can use one of the following to create a tag policy:
+ AWS CLI: [aws organizations create\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html)

  1. Create an AI services opt\-out policy like the following, and store it in a text file\. Note that "`optOut`" and "`optIn`" are case\-sensitive\.

     ```
     {
         "services": {
             "default": {
                 "opt_out_policy": {
                     "@@assign": "optOut"
                 }
             },
             "rekognition": {
                 "opt_out_policy": {
                     "@@assign": "optIn"
                 }
             }
         }
     }
     ```

     This AI services opt\-out policy specifies that all accounts affected by the policy are opted out of all AI services except for Amazon Rekognition\. 

  1. Import the JSON policy file to create a new policy in the organization\. In this example, the previous JSON file was named `policy.json`\.

     ```
     $ aws organizations create-policy \
         --type AISERVICES_OPT_OUT_POLICY \
         --name "MyTestPolicy" \
         --description "My test policy" \
         --content file://policy.json
     {
         "Policy": {
             "Content": "{\"services\":{\"default\":{\"opt_out_policy\":{\"@@assign\":\"optOut\"}},\"rekognition\":{\"opt_out_policy\":{\"@@assign\":\"optIn\"}}}}",
             "PolicySummary": {
                 "Id": "p-i9j8k7l6m5"
                 "Arn": "arn:aws:organizations::o-aa111bb222:policy/aiservices_opt_out_policy/p-i9j8k7l6m5",
                 "Description": "My test policy",
                 "Name": "MyTestPolicy",
                 "Type": "AISERVICES_OPT_OUT_POLICY"
             }
         }
     }
     ```
+ AWS SDKs: [CreatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreatePolicy.html)

------

**What to do next**  
After you create an AI services opt\-out policy, you can put your opt\-out choices into effect\. To do that, you can [attach the policy](attach-tag-policy.md) to the organization root, organizational units \(OUs\), AWS accounts within your organization, or a combination of all of those\. 

## Updating an AI services opt\-out policy<a name="update-ai-opt-out-policy-procedure"></a>

**Minimum permissions**  
To update an AI services opt\-out policy, you must have permission to run the following actions:  
`organizations:UpdatePolicy` with a `Resource` element in the same policy statement that includes the ARN of the specified policy \(or "\*"\)
`organizations:DescribePolicy` with a `Resource` element in the same policy statement that includes the Amazon Resource Name \(ARN\) of the specified policy \(or "\*"\)

------
#### [ Old console ]

**To update an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/home#/policies/ai-services)** page, choose the policy that you want to update, and then in the details pane choose **View details**\.

1. On the policy's detail page, choose **Edit policy**\.

1. You can enter a new **Policy name**, **Policy description**, or edit the **JSON** policy text\. For information about AI services opt\-out policy syntax, see [AI services opt\-out policy syntax and examples](orgs_manage_policies_ai-opt-out_syntax.md)\. For example policies that you can use as a starting point, see [AI services opt\-out policy examples](orgs_manage_policies_ai-opt-out_syntax.md#ai-opt-out-policy-examples)\.

1. When you're finished updating the policy, choose **Save changes**\.

------
#### [ New console ]

**To update an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy)** page, choose the name of the policy that you want to update\.

1. On the policy's detail page, choose **Edit policy**\.

1. You can enter a new **Policy name**, **Policy description**, or edit the **JSON** policy text\. For information about AI services opt\-out policy syntax, see [AI services opt\-out policy syntax and examples](orgs_manage_policies_ai-opt-out_syntax.md)\. For example policies that you can use as a starting point, see [AI services opt\-out policy examples](orgs_manage_policies_ai-opt-out_syntax.md#ai-opt-out-policy-examples)\.

1. When you're finished updating the policy, choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To update a policy**  
You can use one of the following to update a policy: 
+ AWS CLI: [aws organizations update\-policy](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-policy.html)

  The following example renames an AI services opt\-out policy\.

  ```
  $  aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --name "Renamed policy"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/aiservices_opt_out_policy/p-i9j8k7l6m5",
              "Name": "Renamed policy",
              "Type": "AISERVICES_OPT_OUT_POLICY",
              "AwsManaged": false
          },
          "Content": "{\"services\":{\"default\":{\"opt_out_policy\":   ....TRUNCATED FOR BREVITY...   :{\"@@assign\":\"optIn\"}}}}"
      }
  }
  ```

  The following example adds or changes the description for an AI services opt\-out policy\.

  ```
  $  aws organizations update-policy \
      --policy-id p-i9j8k7l6m5 \
      --description "My new description"
  {
      "Policy": {
          "PolicySummary": {
              "Id": "p-i9j8k7l6m5",
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/aiservices_opt_out_policy/p-i9j8k7l6m5",
              "Name": "Renamed policy",
              "Description": "My new description",
              "Type": "AISERVICES_OPT_OUT_POLICY",
              "AwsManaged": false
          },
          "Content": "{\"services\":{\"default\":{\"opt_out_policy\":   ....TRUNCATED FOR BREVITY...   :{\"@@assign\":\"optIn\"}}}}"
      }
  }
  ```

  The following example changes the JSON policy document attached to an AI services opt\-out policy\. In this example, the content is taken from a file called `policy.json` with the following text:

  ```
  {
      "services": {
          "default": {
              "opt_out_policy": {
                  "@@assign": "optOut"
              }
          },
          "comprehend": {
              "opt_out_policy": {
                  "@@operators_allowed_for_child_policies": ["@@none"],
                  "@@assign": "optOut"
              }
          },
          "rekognition": {
              "opt_out_policy": {
                  "@@assign": "optIn"
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
              "Arn": "arn:aws:organizations::123456789012:policy/o-aa111bb222/aiservices_opt_out_policy/p-i9j8k7l6m5",
              "Name": "Renamed policy",
              "Description": "My new description",
              "Type": "AISERVICES_OPT_OUT_POLICY",
              "AwsManaged": false
          },
           "Content": "{\n\"services\": {\n\"default\": {\n\"   ....TRUNCATED FOR BREVITY....    ": \"optIn\"\n}\n}\n}\n}\n"}
  }
  ```
+ AWS SDKs: [UpdatePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdatePolicy.html)

------

## Editing tags attached to an AI services opt\-out policy<a name="tag-ai-opt-out-policy-procedure"></a>

When you sign in to your organization's management account, you can add or remove the tags attached to an AI services opt\-out policy\. For more information about tagging, see [Tagging AWS Organizations resources](orgs_tagging.md)\.

**Minimum permissions**  
To edit the tags attached to an AI services opt\-out policy in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization`– required only when using the Organizations console
`organizations:DescribePolicy`– required only when using the Organizations console
`organizations:TagResource`
`organizations:UntagResource`

------
#### [ Old console ]

**To edit the tags attached to an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/home#/policies/ai-services)** page, choose the policy with the tags that you want to edit\.

1. In the details pane on the right, under **Tags**, choose **Edit tags**\.

1. You can perform any of these actions on this page:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ New console ]

**To edit the tags attached to an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy)** page, choose the name of the policy with the tags that you want to edit\.

1. On the chosen policy's detail page, choose the **Tags** tab, and then choose **Manage tags**\.

1. You can perform any of these actions on this page:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove**\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ AWS CLI & AWS SDKs ]

**To edit the tags attached to a AI services opt\-out policy**  
You can use one of the following commands to edit the tags attached to a AI services opt\-out policy:
+ AWS CLI: [aws organizations tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [aws organizations untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------

## Deleting an AI services opt\-out policy<a name="delete-ai-opt-out-policy-procedure"></a>

When you sign in to your organization's management account, you can delete a policy that you no longer need in your organization\. 

Before you can delete a policy, you must first detach it from all attached entities\.

**Minimum permissions**  
To delete a policy, you must have permission to run the following action:  
`organizations:DescribePolicy` \(console only – to navigate to the policy\)
`organizations:DeletePolicy`

------
#### [ Old console ]

**To delete an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/home#/policies/ai-services)** page, choose the policy that you want to delete\.

1. You must first detach the policy that you want to delete from all roots, OUs, and accounts\. In the details pane on the right, choose each of the **Accounts**, **Organizational units**, and **Roots** sections\. If any accounts, OUs, or Roots indicate that the policy is attached, choose **Detach**\. Repeat until you remove all targets\.

1. With the policy still selected, choose **Delete** above the list of policies\.

------
#### [ New console ]

**To delete an AI services opt\-out policy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\. 

1. On the **[AI services opt\-out policies](https://console.aws.amazon.com/organizations/v2/home/policies/aiservices-opt-out-policy)** page, choose the name of the policy that you want to delete\.

1. You must first detach the policy that you want to delete from all roots, OUs, and accounts\. Choose the **Targets** tab, choose the radio button next to each root, OU, or account that is shown in the **Targets** list, and then choose **Detach**\. In the confirmation dialog box, choose **Detach**\. Repeat until you remove all targets\.

1. Choose **Delete** at the top of the page\.

1. On the confirmation dialog box, enter the name of the policy, and then choose **Delete**\.

------
#### [ AWS CLI & AWS SDKs ]

**To delete an AI services opt\-out policy**  
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