# AI services opt\-out policy syntax and examples<a name="orgs_manage_policies_ai-opt-out_syntax"></a>

This topic describes Artificial Intelligence \(AI\) services opt\-out policy syntax and provides examples\.

## Syntax for AI services opt\-out policies<a name="ai-opt-out-policy-syntax-reference"></a>

An AI services opt\-out policy is a plaintext file that is structured according to the rules of [JSON](http://json.org)\. The syntax for AI services opt\-out policies follows the syntax for management policy types\. For a complete discussion of that syntax, see [Policy syntax and inheritance for management policy types](orgs_manage_policies_inheritance_mgmt.md)\. This topic focuses on applying that general syntax to the specific requirements of the AI services opt\-out policy type\.

The following policy shows the basic AI services opt\-out policy syntax\. If this example was attached directly to an account, that account would be explicitly opted out of one service and opted in to another\. Other services could be opted in or opted out by policies inherited from higher levels \(OU or root policies\)\.

```
{
    "services": {
        "rekognition": {
            "opt_out_policy": {
                "@@assign": "optOut"
            }
        },
        "lex": {
            "opt_out_policy": {
                "@@assign": "optIn"
            }
        }
    }
}
```

Imagine the following example policy attached to the organization's root\. It sets the default for the organization to opt out of all AI services\. This automatically includes any AI services not otherwise explicitly exempted, including any AI services that AWS might deploy in the future\. You can attach child policies to OUs or directly to accounts to override this setting for any AI service except Amazon Comprehend\. The second entry in the following example uses `@@operators_allowed_for_child_policies` set to `none` to prevent it from being overridden\. The third entry in the example makes an organization\-wide exemption for Amazon Rekognition\. It opts in the entire organization for that service, but the policy does allow child policies to override where appropriate\.

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

AI services opt\-out policy syntax includes the following elements: 
+ The `services` element\. An AI services opt\-out policy is identified by this fixed filed name name as the outermost JSON containing element\.

  An AI services opt\-out policy can have one or more statements under the `services` element\. Each statement contains the following elements: 
  + A *service name key* that identifies and AWS AI service\. The following key names are the valid values for this field:
    + **`default`** – represents **all** currently available AI services and implicitly includes any AI services that might be added in the future\.
    + `codeguruprofiler`
    + `comprehend`
    + `lex`
    + `polly`
    + `rekognition`
    + `textract`
    + `transcribe`
    + `translate`

    Each policy statement identified by a service name key can contain the following elements:
    + The `opt_out_policy` key\. This key must be present\. This is the only key you can place under a service name key\.

      The `opt_out_policy` key can contain ***only*** the `@@assign` operator with one of the following values:
      + `optOut` – you choose to opt out of content use for the specified AI service\.
      + `optIn` – you choose to opt in to content use for the specified AI service\.
**Notes**  
You can't use the `@@append` and `@@remove` inheritance operators in AI services opt\-out policies\.
You can't use the `@@enforced_for` operator in AI services opt\-out policies\.
  + At any level, you can specify the `@@operators_allowed_for_child_policies` operator to control what child policies can do to override settings imposed by parent policies\. You can specify one of the following values:
    + `@@assign` – child policies of this policy can use the `@@assign` operator to override the inherited value with a different value\.
    + `@@none` – child policies of this policy can't change the value\.

    The behavior of the `@@operators_allowed_for_child_policies` depends on where you place it\. You can use the following locations:
    + Under the `services` key – controls whether a child policy can add to or change the list of services in the effective policy\.
    + Under the key for a specific AI service or the `default` key \- controls whether a child policy can add to or change the list of keys under this specific entry\.
    + Under the `opt_out_policies` key for a specific service – controls whether a child policy can change only the setting for this specific service\.

## AI services opt\-out policy examples<a name="ai-opt-out-policy-examples"></a>

The example policies that follow are for information purposes only\.

### Example 1: Opt out of all AI services for all accounts in the organization<a name="ai-opt-out-policy-example-1"></a>

The following example shows a policy that you could attach to your organization's root to opt out of AI services for accounts in your organization\. 

**Tip**  
If you copy the following example using the copy button in the example's upper\-right corner, the copy doesn't include the line numbers\. It's ready to paste\.

```
    | {
    |     "services": {
[1] |         "@@operators_allowed_for_child_policies": ["@@none"],
    |         "default": {
[2] |             "@@operators_allowed_for_child_policies": ["@@none"],
    |             "opt_out_policy": {
[3] |                 "@@operators_allowed_for_child_policies": ["@@none"],
    |                 "@@assign": "optOut"
    |             }
    |         }
    |     }
    | }
```
+ \[1\] – The `"@@operators_allowed_for_child_policies": ["@@none"]` that is under `services` prevents any child policy from adding any new sections for individual services other than the `default` section that is already there\. `Default` is the placeholder that represents "all AI services"\.
+ \[2\] – The `"@@operators_allowed_for_child_policies": ["@@none"]` that is under `default` prevents any child policy from adding any new sections other than the `opt_out_policy` section that is already there\.
+ \[3\] – The `"@@operators_allowed_for_child_policies": ["@@none"]` that is under `opt_out_policy` prevents child policies from changing the value of the `optOut` setting or adding any additional settings\. 

### Example 2: Set an organization default setting for all services, but allow child policies to override the setting for individual services<a name="ai-opt-out-policy-example-2"></a>

The following example policy sets an organization\-wide default for all AI services\. The value for `default` prevents a child policy from change the `optOut` value for service `default`, the placeholder for all AI services\. If this policy is applied as a parent policy by attaching it to the root or to an OU, child policies can still change the opt\-out setting for individual services, as shown in the second policy\.
+ Because there is no `"@@operators_allowed_for_child_policies": ["@@none"]` under the `services` key, child policies can add new sections for individual services\.
+ The `"@@operators_allowed_for_child_policies": ["@@none"]` that is under `default` prevents any child policy from adding any new sections other than the `opt_out_policy` section that is already there\.
+ The `"@@operators_allowed_for_child_policies": ["@@none"]` that is under `opt_out_policy` prevents child policies from changing the value of the `optOut` setting or adding any additional settings\. 

**Organization root AI services opt\-out parent policy**

```
{
    "services": {
        "default": {
            "@@operators_allowed_for_child_policies": ["@@none"],
            "opt_out_policy": {
                "@@operators_allowed_for_child_policies": ["@@none"],
                "@@assign": "optOut"
            }
        }
    }
}
```

The following example policy assumes that the previous example policy is attached to either the organization root or to a parent OU, and that you attach this example to an account affected by the parent policy\. It overrides the default opt\-out setting and explicitly opts in to only the Amazon Lex service\.

**AI services opt\-out child policy**

```
{
    "services": {
        "lex": {
            "opt_out_policy": {
                "@@assign": "optIn"
            }
        }
    }
}
```

The resulting effective policy for the AWS account is that the account opts in to only Amazon Lex, and opts out of all other AWS AI services because of the inherited `default` opt\-out setting from the parent policy\.

### Example 3: Define an organization\-wide AI services opt\-out policy for a single service<a name="ai-opt-out-policy-example-3"></a>

The following example shows an AI services opt\-out policy that defines an `optOut` setting for a single AI service\. If this policy is attached to the organization's root, it prevents any child policy from overriding the `optOut` setting for this one service\. Other services are not addressed by this policy, but could be affected by child policies in other OUs or accounts\.

```
{
    "services": {
        "rekognition": {
            "opt_out_policy": {
                "@@assign": "optOut",
                "@@operators_allowed_for_child_policies": ["@@none"]
            }
        }
    }
}
```