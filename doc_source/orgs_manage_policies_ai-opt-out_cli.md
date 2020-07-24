# Using the AWS CLI to manage AI services opt\-out policies<a name="orgs_manage_policies_ai-opt-out_cli"></a>

Before you can use Artificial Intelligence \(AI\) services opt\-out policies, you must enable the policy type in your organization\.

## Enabling AI services opt\-out policies for your organization<a name="ai-opt-out-policy-enable-cli"></a>

Enabling the use of AI services opt\-out policies is a one\-time task\. You enable AI services opt\-out policies on the organization root, even if you plan to attach AI services opt\-out policies to individual accounts only\. 

**To enable AI services opt\-out policies**

1. Find the ID of the root for your organization so you can specify where to enable the AI services opt\-out policy type\. To find the root ID, run the following from a command prompt\. This example shows sample output\.

   ```
   $ aws organizations list-roots
   {
       "Roots": [
           {
               "Id": "r-examplerootid111",
               "Arn": "arn:aws:organizations::123456789012:root/o-exampleorgid/r-examplerootid111",
               "Name": "Root",
               "PolicyTypes": [
                   {
                       "Type": "SERVICE_CONTROL_POLICY",
                       "Status": "ENABLED"
                   },
                   {
                       "Type": "TAG_POLICY",
                       "Status": "ENABLED"
                   }
               ]
           }
       ]
   }
   ```

   In this example, `r-examplerootid111` is the root ID of the organization\. Use the root ID of your own organization in the next step\.

1. Run the following to enable AI services opt\-out policies for the specified root in your organization\.

   ```
   $ aws organizations enable-policy-type \
       --policy-type AISERVICES_OPT_OUT_POLICY \
       --root-id r-examplerootid111
   {
       "Root": {
           "Id": "r-examplerootid111",
           "Arn": "arn:aws:organizations::111111111111:root/o-exampleorgid/r-examplerootid111",
           "Name": "Root",
           "PolicyTypes": [
               {
                   "Type": "SERVICE_CONTROL_POLICY",
                   "Status": "ENABLED"
               },
               {
                   "Type": "TAG_POLICY",
                   "Status": "ENABLED"
               },
               {
                   "Type": "AISERVICES_OPT_OUT_POLICY",
                   "Status": "ENABLED"
               }
           ]
       }
   }
   ```

   This command enables AI services opt\-out policies for the organization with the root ID `r-examplerootid111.` 

## Creating an AI services opt\-out policy<a name="ai-opt-out-create-first-cli"></a>

After you enable AI services opt\-out policies, you're ready to create your first policy of that type\. 

You can use any basic text editor to create a tag policy\. Use JSON syntax and save the policy as a file with any name and extension in a location of your choosing\. AI services opt\-out policies can have a maximum of 2,500 characters, including spaces\. 

**To create an AI services opt\-out policy**

1. Create an AI services opt\-out policy like the following, and store it in a text file\.

   ```
   {
       "services": {
           "default": {
               "@@assign": "OptOut"
           },
           "rekognition": {
               "@@assign": "OptIn"
           }
       }
   }
   ```

   This AI services opt\-out policy specifies that all accounts affected by the policy are opted out of all AI services except for Amazon Rekognition\. 

1. Import the JSON policy file to create a new policy in the organization\. In this example, the previous JSON file was named `policy.json`\.

   ```
   $ aws organizations create-policy \
       --type AI_OPT_OUT_POLICY \
       --name "MyTestPolicy" \
       --description "My test policy" \
       --content file://policy.json
   {
       "Policy": {
           "Content": "{\"service\":{\"default\":{\"@@assign\":\"OptOut\"},\"rekognition\":{\"@@assign\":\"OptIn\"}}}",
           "PolicySummary": {
               "Arn": "arn:aws:organizations::o-exampleorgid:policy/ai_opt_out_policy/p-examplepolicyid123",
               "Description": "My test policy",
               "Name": "MyTestPolicy",
               "Type": "AI_OPT_OUT_POLICY"
           }
       }
   }
   ```

## Attach the AI services opt\-out policy to a root, OU, or account<a name="ai-opt-out-attach-first-cli"></a>

After you create an AI services opt\-out policy, you're ready to attach it to your organization root, an organizational unit \(OU\), or an individual account\. Attaching an AI services opt\-out policy to the organization root affects all of your organization's member accounts\. When you attach an AI services opt\-out policy to an individual account, only that account is subject to the policy\. The account is still subject to any AI services opt\-out policy that is attached to the organization root or to any OUs the account is contained in\.

The following procedure shows how to attach the AI services opt\-out policy you just created to a single test account\.
+ Attach the AI services opt\-out policy to your test account by running a command like the following\.

  ```
  $ aws organizations attach-policy \
      --target-id 123456789012 \
      --policy-id p-examplepolicyid123
  ```

## Determining the effective AI services opt\-out policy for an account<a name="ai-opt-out-get-effective-cli"></a>

To determine what AI services opt\-out policies apply to an account, run the following command from the account and save the results to a file\.

```
$ aws organizations describe-effective-policy
{
    "EffectivePolicy": {
        "PolicyContent": "{\"service\":{\"default\":{\"@@assign\":\"OptOut\"},\"rekognition\":{\"@@assign\":\"OptIn\"}}}",
        "LastUpdatedTimestamp": "2020-05-07T15:18:44.404000-07:00",
        "TargetId": "123456789012,
        "PolicyType": "AI_OPT_OUT_POLICY"
    }
}
```

If AI services opt\-out policies are attached to the account as well as to the organization root or an OU the account is in, the combination of all applicable policies defines the account's effective policy\. In these cases, running `describe-effective-policy` from the account returns the contents of the combined policy\. 